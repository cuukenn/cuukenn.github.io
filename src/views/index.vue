<template>
  <div id="main" ref="MainRef">
    <canvas id="canvas" ref="CanvasRef"></canvas>
  </div>
</template>
<script setup lang="ts">
import { ref, onMounted } from 'vue'
const MainRef = ref()
const CanvasRef = ref()
let ctx: any
let count: number
// 定义一个粒子数组
const particlesArray: Array<Particle> = []
// 定义粒子类
class Particle {
  public x: number
  public y: number
  public directionX: number
  public directionY: number
  constructor(x: any, y: any) {
    this.x = x
    this.y = y
    // x，y轴的移动速度  -0.5 -- 0.5
    this.directionX = Math.random() - 0.5
    this.directionY = Math.random() - 0.5
  }
  // 更新点的坐标
  update() {
    this.x += this.directionX
    this.y += this.directionY
  }
  // 绘制粒子
  draw() {
    ctx.beginPath()
    ctx.arc(this.x, this.y, 2, 0, Math.PI * 2)
    ctx.closePath()
    ctx.fillStyle = 'white'
    ctx.fill()
  }
}
// 创建粒子
const createParticle = () => {
  // 生成一个点的随机坐标
  let x = Math.random() * innerWidth
  let y = Math.random() * innerHeight
  particlesArray.push(new Particle(x, y))
}
// 处理粒子
// 先更新坐标，再绘制出来
const handleParticle = () => {
  for (let i = 0; i < particlesArray.length; i++) {
    let particle = particlesArray[i]
    particle.update()
    particle.draw()
    // 超出范围就将这个粒子删除
    if (particle.x < 0 || particle.x > CanvasRef.value.width || particle.y < 0 || particle.y > CanvasRef.value.height) {
      particlesArray.splice(i, 1)
    }
    // 绘制两个点之间的连线
    for (let j = i + 1; j < particlesArray.length; j++) {
      const dx = particlesArray[j].x - particlesArray[i].x
      const dy = particlesArray[j].y - particlesArray[i].y
      const dist = Math.sqrt(Math.pow(dx, 2) + Math.pow(dy, 2))
      if (dist < 100) {
        ctx.beginPath()
        ctx.strokeStyle = 'rgba(255, 255, 255, ' + (1 - dist / 100)
        ctx.moveTo(particlesArray[i].x, particlesArray[i].y)
        ctx.lineTo(particlesArray[j].x, particlesArray[j].y)
        ctx.closePath()
        ctx.lineWidth = 1
        ctx.stroke()
      }
    }
  }
}
const draw = () => {
  // 首先清空画布
  ctx.clearRect(0, 0, CanvasRef.value.width, CanvasRef.value.height)
  // 如果粒子数量小于规定数量，就生成新的粒子
  if (particlesArray.length < count) {
    createParticle()
  }
  // 处理粒子
  handleParticle()
}
const initCanvas = () => {
  const width = CanvasRef.value.offsetWidth
  const height = CanvasRef.value.offsetHeight
  // 定义页面内粒子的数量
  count = parseInt(((width / 80) * height) / 80 + '')
  //初始化ctx
  ctx = CanvasRef.value.getContext('2d')
}
onMounted(() => {
  //初始化
  initCanvas()
  // 设置动画帧
  //setInterval(draw, 10)
  requestAnimationFrame(function fn() {
    draw()
    requestAnimationFrame(fn)
  })
})
</script>
<style scoped>
#main {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: red;
}
#main {
  background: url('https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fi0.hdslb.com%2Fbfs%2Farticle%2Fde2e1aded449e5550aa8eaeb29a0d6b2f8ba7395.jpg&refer=http%3A%2F%2Fi0.hdslb.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1664956984&t=58239fc59003982cc1eeed5aa58db976');
  background-size: cover;
  opacity: 0.8;
}
#canvas {
  display: block;
  width: 100%;
  height: 100%;
  z-index: -1;
}
</style>
