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
          <div class="item-btn">点击进入</div>
        </div>
      </div>
    </div>
  </div>
</template>
<script lang="ts" setup>
import { onMounted, onUnmounted, ref, Ref } from 'vue'
interface GalleryItems {
  src?: string
  url?: string
  name?: string
}
const w = ref('1560px')
const galleryItems: Ref<Array<GalleryItems>> = ref([
  {
    src: 'https://www.idodx.com/static/picture/1.jpg',
  },
  {
    src: 'https://www.idodx.com/static/picture/2.jpg',
  },
  {
    src: 'https://www.idodx.com/static/picture/3.jpg',
  },
  {
    src: 'https://www.idodx.com/static/picture/4.jpg',
  },
  {
    src: 'https://www.idodx.com/static/picture/1.jpg',
  },
  {
    src: 'https://www.idodx.com/static/picture/2.jpg',
  },
])
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
