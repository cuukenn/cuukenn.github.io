<template>
  <div class="container-header">
    <section>个人网址导航</section>
    <section></section>
  </div>
</template>
<script lang="ts" setup></script>
<style scoped>
.container-header {
  display: flex;
  align-items: center;
  padding: 5px 0px;
  width: 100%;
  justify-content: space-between;
  color: white;
  padding-top: 40px;
}
.container-footer > * {
  display: inline-block;
}
.container-header > *:first-child {
  width: 100%;
  margin-left: 80px;
  font-size: 20px;
}
.container-header > *:last-child {
  width: 100px;
  margin-right: 80px;
}
</style>
