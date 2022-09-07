<template>
  <div class="container">
    <Particles id="tsparticles" :options="options" :particlesInit="particlesInit" />
    <header class="header">
      <x-header></x-header>
    </header>
    <main class="main">
      <x-main></x-main>
    </main>
    <footer class="footer">
      <x-footer></x-footer>
    </footer>
  </div>
</template>
<script setup lang="ts">
import type { Engine } from 'tsparticles-engine'
import { loadFull } from 'tsparticles'
import XHeader from './header/index.vue'
import XMain from './main/index.vue'
import XFooter from './footer/index.vue'
const options = {
  background: {
    color: {
      value: 'white',
    },
  },
  fpsLimit: 120,
  interactivity: {
    events: {
      onClick: {
        enable: true,
        mode: 'push',
      },
      onHover: {
        enable: true,
        mode: 'repulse',
      },
      resize: true,
    },
    modes: {
      bubble: {
        distance: 400,
        duration: 2,
        opacity: 0.8,
        size: 40,
        speed: 3,
      },
      push: {
        quantity: 4,
      },
      repulse: {
        distance: 200,
        duration: 0.4,
      },
    },
  },
  particles: {
    color: {
      value: '#F7F7F7',
    },
    links: {
      color: '#F7F7F7',
      distance: 150,
      enable: true,
      opacity: 0.5,
      width: 1,
    },
    collisions: {
      enable: false,
    },
    move: {
      direction: 'none',
      enable: true,
      outMode: 'bounce',
      random: false,
      speed: 6,
      straight: false,
    },
    number: {
      density: {
        enable: true,
        value_area: 800,
      },
      value: 80,
    },
    opacity: {
      value: 0.5,
    },
    shape: {
      type: 'circle',
    },
    size: {
      random: true,
      value: 5,
    },
  },
  detectRetina: true,
}
const particlesInit = async (engine: Engine) => {
  await loadFull(engine)
}
</script>
<style scoped>
.container {
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: column;
  /* 父元素一定要给高度 */
  height: 100vh;
}
.container {
  background-image: url(https://www.idodx.com/static/image/background-7.jpg);
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
  background-blend-mode: overlay;
  animation: background-fade 5s forwards;
}
@keyframes background-fade {
  70% {
    background-color: rgba(0, 0, 0, 0);
  }
  100% {
    background-color: rgba(0, 0, 0, 0.7);
  }
}
.header {
  width: 100%;
  height: 100px;
}
.main {
  width: 100%;
  flex: 1;
}
.footer {
  width: 100%;
  height: 40px;
}
</style>
