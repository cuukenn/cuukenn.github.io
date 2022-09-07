<template>
  <div class="header-content-layout">
    <div class="main-content">
      <div class="item-layout">
        <header></header>
        <main><p>&copy;changgg</p></main>
        <footer>
          <p>山止川行，风禾尽起</p>
        </footer>
      </div>
    </div>
  </div>
</template>
<script lang="ts" setup></script>
<style scoped>
.header-content-layout {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  color: white;
}
.header-content-layout > main-content {
  height: 90%;
  background-color: rebeccapurple;
}
/* 内容布局 */
.item-layout {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
}
.item-layout > header {
  flex-shrink: 0;
}
.item-layout > main {
  flex-grow: 2;
}
.item-layout > footer {
  height: 100px;
  width: 80%;
  flex-shrink: 0;
}
/* header头像 */
.item-layout > header {
  /* 固定图片位置 */
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
  background-image: url(https://cuukenn.github.io/personal-blog/images/avatar.png);
  height: 140px;
  width: 140px;
  border-radius: 50%;
  border: 2px solid rgb(47, 138, 191);
  animation: rotate 10s linear infinite;
}
/* 图片旋转 */
@keyframes rotate {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(359deg);
  }
}
/* 文字居中 */
.item-layout > main,
footer {
  margin-top: 10px;
  text-align: center;
}
.item-layout > footer {
  width: max-content;
}
</style>
