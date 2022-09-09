<template>
  <div ref="GalleryMainRef" class="gallery-main">
    <header ref="GalleryHeaderRef" @mouseenter.stop="parantMousIn" @mouseleave.stop="parantMousOut"></header>
    <main>
      <div ref="GalleryContainerRef" class="gallery-container" :style="{ left: left + 'px' }">
        <gallery-item
          v-for="(item, index) in items"
          :key="index"
          :url="item.url"
          :src="item.src"
          :name="item.name"
          @mouseenter.stop="childMousIn"
          @mouseleave="childMousOut"
        ></gallery-item>
      </div>
    </main>
  </div>
</template>
<script lang="ts" setup>
import { ref, onMounted, onUnmounted } from 'vue'
import GalleryItem from './gallery-item.vue'
interface Items {
  src?: string
  url?: string
  name?: string
}
const props = defineProps({
  items: Array<Items>,
})
const left = ref(0)
const GalleryContainerRef = ref()
const GalleryMainRef = ref()
const GalleryHeaderRef = ref()
//0:auto,1:mouse,2:stop
const autoRunFlag = ref(0)
let galleryWidth: number, mainWidth: number, eachItemWidth: number
let rafId: any, finished: boolean, currentIndex: number, isLeft: boolean
const offsetLefts: Array<number> = []
const parantMousIn = () => {
  autoRunFlag.value = 1
}
const parantMousOut = () => {
  autoRunFlag.value = 0
}
const childMousIn = () => {
  autoRunFlag.value = 2
}
const childMousOut = () => {
  autoRunFlag.value = 0
}
//鼠标移动检测函数
const mouseMoveTrigger = (e: any) => {
  const leftInstance = e.layerX
  //更新位置
  const fn = (x: number) => {
    const percent = x / mainWidth
    const widthTmp = galleryWidth * percent
    currentIndex = Math.floor(widthTmp / eachItemWidth)
  }
  fn(leftInstance)
}
//计算各个子项居中的偏移值
const initOffsetLefts = (width: number, eachWidth: number) => {
  if (props.items === undefined) {
    return
  }
  const len = props.items.length
  offsetLefts[0] = (width - eachWidth) >> 1
  for (let i = 1; i < len; i++) {
    offsetLefts[i] = offsetLefts[i - 1] - eachWidth
  }
}
//移动offset
const requestAnimationFrameAction = () => {
  //当需要进行位置调整时
  const move = (step: number) => {
    const exceptedOffset = offsetLefts[currentIndex]
    if (left.value > exceptedOffset) {
      if (left.value - step > exceptedOffset) {
        left.value = left.value - step
      }
    } else if (left.value < exceptedOffset) {
      left.value = left.value + step
      if (left.value + step < exceptedOffset) {
        left.value = left.value + step
      }
    }
  }
  //自动滚动
  const moveForMouseOut = () => {
    const step = 4
    if (!isLeft && left.value <= offsetLefts[0]) {
      left.value += step
      if (left.value >= offsetLefts[0]) {
        isLeft = true
      }
    } else {
      left.value -= step
      if (left.value <= offsetLefts[offsetLefts.length - 1]) {
        isLeft = false
      }
    }
  }
  if (autoRunFlag.value == 0) {
    moveForMouseOut()
  } else {
    if (autoRunFlag.value === 1) {
      //鼠标滑动控制
      move(16)
    }
  }
  if (finished) {
    //不再需要动画，直接退出
    return
  }
  rafId = requestAnimationFrame(requestAnimationFrameAction)
}
onMounted(() => {
  const getWidth = (div: any) => {
    return div.style.width || div.clientWidth || div.offsetWidth || div.scrollWidth
  }
  galleryWidth = getWidth(GalleryContainerRef.value)
  mainWidth = getWidth(GalleryMainRef.value)
  const itemSize = props.items != undefined ? props.items.length : 0
  if (itemSize <= 0) {
    return
  }
  //计算每个子项的宽度、从而计算合适的最大最小偏移位置
  eachItemWidth = galleryWidth / itemSize
  requestAnimationFrameAction()
  initOffsetLefts(mainWidth, eachItemWidth)
  GalleryHeaderRef.value.addEventListener('mousemove', mouseMoveTrigger)
})
onUnmounted(() => {
  GalleryHeaderRef.value.removeEventListener('mousemove', mouseMoveTrigger)
  finished = true
})
</script>
<style scoped>
.gallery-main {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-items: center;
  position: relative;
  overflow: hidden;
}
.gallery-container {
  width: max-content;
  height: 260px;
  margin: 0 1%;
  position: absolute;
}
/* 内容布局 */
.gallery-main {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
}
.gallery-main > header {
  height: 20%;
  flex-shrink: 0;
  width: 100%;
}
.gallery-main > main {
  flex-grow: 2;
}
</style>
