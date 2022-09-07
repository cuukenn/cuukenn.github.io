<template>
  <Gallery :items="galleryItems"></Gallery>
</template>
<script lang="ts" setup>
import Gallery from './components/gallery/index.vue'
const galleryItems: Array<any> = [
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
    url: 'https://cuukenn.github.io/personal-blog',
  },
  {
    src: 'https://img2.baidu.com/it/u=1023460905,1968026395&fm=253&fmt=auto&app=138&f=JPEG?w=500&h=313',
    name: '个人x-admin管理端',
    url: 'https://cuukenn.github.io/x-admin-web',
  },
  {
    src: 'https://img2.baidu.com/it/u=3107292621,3609358841&fm=253&fmt=auto&app=138&f=JPEG?w=923&h=500',
  },
]
</script>
<style scoped></style>
