<template>
  <div class="gallery-main">
    <div ref="GalleryContainerRef" class="gallery-container" :style="{ left: left + 'px' }">
      <gallery-item
        v-for="(item, index) in items"
        :key="index"
        :url="item.url"
        :src="item.src"
        :name="item.name"
        @mouseover="() => (flag = true)"
        @mouseout="() => (flag = false)"
      ></gallery-item>
    </div>
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
let moveStep = 0
let lastMoveSetp = moveStep
let maxLeft = 0
let minLeft = 0
const mouseMoveTrigger = (e: any) => {
  const leftInstance = e.screenX
  const screenWidth = window.innerWidth
  lastMoveSetp = moveStep
  if (leftInstance < screenWidth / 2) {
    moveStep = 8
  } else {
    moveStep = -8
  }
  if (changePositionId != undefined && lastMoveSetp === moveStep) {
    return
  }
  changePosition()
}
let changePositionId: any
const changePosition = () => {
  changePositionId = requestAnimationFrame(function fn() {
    if (flag.value) {
      clearTimeout(changePositionId)
      changePositionId = undefined
      return
    }
    if (moveStep !== 0) {
      if (left.value < minLeft && moveStep < 0) {
        clearTimeout(changePositionId)
        changePositionId = undefined
        return
      } else if (left.value > maxLeft && moveStep > 0) {
        clearTimeout(changePositionId)
        changePositionId = undefined
        return
      } else {
        left.value += moveStep
      }
    } else {
      clearTimeout(changePositionId)
      changePositionId = undefined
    }
    changePositionId = requestAnimationFrame(fn)
  })
}
const flag = ref(false)
onMounted(() => {
  const div = GalleryContainerRef.value
  const galleryWidth = div.style.width || div.clientWidth || div.offsetWidth || div.scrollWidth
  const itemSize = props.items != undefined ? props.items.length : 0
  if (itemSize <= 0) {
    return
  }
  //计算每个子项的宽度、从而计算合适的最大最小偏移位置
  const eachItem = galleryWidth / itemSize
  const eachItemHalf = eachItem >> 1
  //设置左右移动距离
  minLeft = -1 * (galleryWidth - document.body.clientWidth + eachItemHalf)
  maxLeft = eachItemHalf
  window.addEventListener('mousemove', mouseMoveTrigger)
})
onUnmounted(() => {
  window.removeEventListener('mousemove', mouseMoveTrigger)
})
</script>
<style scoped>
.gallery-main {
  width: 98%;
  height: 100%;
  margin: 0 1%;
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
</style>
