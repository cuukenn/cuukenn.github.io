<template>
  <div class="item-container" :style="{ backgroundImage: `url(${src})` }">
    <div class="item-mask"></div>
    <div class="item-layout">
      <header>
        <p style="font-size: 26px; color: aquamarine">
          {{ name ? name : 'TODO' }}
        </p>
      </header>
      <main>
        <div class="item-btn" @click.stop="navigateToUrl(url)">点击进入</div>
      </main>
      <footer></footer>
    </div>
  </div>
</template>
<script lang="ts" setup>
import { defineProps } from 'vue'
const navigateToUrl = (url?: string) => {
  if (url === undefined) {
    return
  }
  window.open(url)
}
defineProps({
  url: String,
  name: String,
  src: String,
})
</script>
<style scoped>
/* 容器 */
.item-container {
  display: inline-block;
  width: 460px;
  height: 260px;
  margin-right: 26px;
  /* 固定图片位置 */
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
  transition: 1.5s;
  /* 用于遮罩层定位 */
  position: relative;
}
/* 遮罩层 */
.item-mask {
  display: none;
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0px;
  left: 0px;
  background-color: rgba(30, 30, 30, 0.4);
}
.item-container:hover > .item-mask {
  display: block;
}
/* 内容布局 */
.item-layout {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
}
.item-layout > header {
  height: 100px;
  flex-shrink: 0;
}
.item-layout > main {
  flex-grow: 2;
}
.item-layout > footer {
  height: 100px;
  flex-shrink: 0;
}
/* header和main内容居中 */
.item-layout > header,
main {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}
/* 拟态按钮 */
.item-btn {
  height: 30px;
  border: 1px solid #fff;
  color: whitesmoke;
  text-align: center;
  line-height: 28px;
  display: block;
  display: none;
  font-size: 20px;
}
.item-container:hover .item-btn {
  display: block;
  z-index: 999;
  cursor: pointer;
}
/* header内容位置 */
.item-layout > header {
  position: relative;
}
.item-layout > header > p {
  position: absolute;
  bottom: 10%;
  color: white;
}
</style>
