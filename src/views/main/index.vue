<template>
  <div class="main-container">
    <div class="gallery-container" :style="{ left: left + 'px' }">
      <div
        class="item"
        :style="{ width: width, backgroundImage: `url(${item.src})` }"
        v-for="(item, index) in galleryItems"
        :key="index"
        @mouseover="() => (flag = true)"
        @mouseout="() => (flag = false)"
      >
        <div class="item-fixed">
          <div class="item-btn" @click.stop="() => toOtherUrl(item.url)">
            点击进入
            <div>{{ item.name ? item.name : '暂无' }}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script lang="ts" setup>
import { it } from 'node:test'
import { onMounted, onUnmounted, ref, Ref } from 'vue'
interface GalleryItems {
  src?: string
  url?: string
  name?: string
}
const w = ref('1560px')
const galleryItems: Ref<Array<GalleryItems>> = ref([
  {
    src: 'https://img0.baidu.com/it/u=3964077488,1615174673&fm=253&fmt=auto&app=138&f=JPG?w=500&h=890',
    name: '个人csdn',
    url: 'https://blog.csdn.net/qq_36759091',
  },
  {
    src: 'https://img2.baidu.com/it/u=2626695872,3823363691&fm=253&fmt=auto&app=138&f=JPEG?w=700&h=500',
    name: '个人gitee',
    url: 'https://www.gitee.com/cuukenn',
  },
  {
    src: 'https://img1.baidu.com/it/u=2843153503,811791573&fm=253&fmt=auto&app=138&f=JPEG?w=1067&h=500',
    name: '个人github',
    url: 'https://www.github.com/cuukenn',
  },
  {
    src: 'https://img2.baidu.com/it/u=3294345311,3250499837&fm=253&fmt=auto&app=138&f=JPEG?w=500&h=275',
    name: '个人博客',
  },
  {
    src: 'https://img2.baidu.com/it/u=1023460905,1968026395&fm=253&fmt=auto&app=138&f=JPEG?w=500&h=313',
    name: '个人x-admin管理端',
    url: 'https://cuukenn.github.io/x-web-admin',
  },
  {
    src: 'https://img2.baidu.com/it/u=3107292621,3609358841&fm=253&fmt=auto&app=138&f=JPEG?w=923&h=500',
  },
])
const toOtherUrl = (url?: string) => {
  if (url !== undefined) {
    window.open(url)
  }
}
const width = ref(260)
const left = ref(0)
const maxLeft = width.value
const minLeft = (width.value * galleryItems.value.length + maxLeft) * -1
let moveStep = 0
let lastMoveSetp = moveStep
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
  window.addEventListener('mousemove', mouseMoveTrigger)
})
onUnmounted(() => {
  window.removeEventListener('mousemove', mouseMoveTrigger)
})
</script>
<style scoped>
.main-container {
  width: 98%;
  height: 100%;
  margin: 0 1%;
  display: flex;
  align-items: center;
  justify-items: center;
  position: relative;
  overflow: hidden;
}
.main-container .gallery-container {
  width: max-content;
  height: 260px;
  margin: 0 1%;
  position: absolute;
}
.gallery-container .item {
  display: inline-block;
  width: 460px;
  height: 260px;
  margin-right: 26px;
  /* 固定图片位置 */
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
  transition: 1.5s;
  position: relative;
}
.gallery-container .item:hover {
  background-size: 126%;
}
.item > .item-fixed {
  display: none;
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0px;
  left: 0px;
  background-color: rgba(30, 30, 30, 0.3);
  display: flex;
  align-items: center;
  justify-items: center;
}
.item-fixed > .item-btn {
  width: 100px;
  height: 30px;
  border: 1px solid #fff;
  color: #fff;
  margin: 85px auto 0px auto;
  text-align: center;
  line-height: 28px;
  display: none;
  cursor: pointer;
}
.item:hover > .item-fixed {
  display: block;
}
.item:hover > .item-fixed > .item-btn {
  display: block;
}
</style>
