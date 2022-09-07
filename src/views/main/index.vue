<template>
  <div class="main-layout">
    <header>
      <header-content></header-content>
    </header>
    <main>
      <main-content></main-content>
    </main>
  </div>
</template>
<script lang="ts" setup>
import MainContent from './components/main/index.vue'
import HeaderContent from './components/header/index.vue'
</script>
<style scoped>
/* 内容布局 */
.main-layout {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
}
.main-layout > header {
  height: 240px;
  flex-shrink: 0;
}
.main-layout > main {
  flex-grow: 2;
}
</style>
