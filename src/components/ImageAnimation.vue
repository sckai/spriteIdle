<script setup lang="ts">
import { onMounted } from 'vue'

import shadowDog from '@/assets/shadow_dog.png'

let canvas
let ctx
let CANVAS_WIDTH
let CANVAS_HEIGHT
let playerImage = new Image()
let frameX = 0
let frameY = 0
let gameFrame = 0
let staggerFrames = 5
let spritStatus = 'idle'
const spriteWidth = 575  // image width(6876) / 12 = 575
const spriteHeigth = 523 // image Height(5230) / 10 = 523
// const spriteAnimations = []
const animationStates = {
  'idle': {
    frames: 7,
    location: []
  },
  'jump': {
    frames: 7,
    location: []
  },
  'fall': {
    frames: 7,
    location: []
  },
  'run': {
    frames: 9,
    location: []
  },
  'dizzy': {
    frames: 11,
    location: []
  },
  'sit': {
    frames: 5,
    location: []
  },
  'roll': {
    frames: 7,
    location: []
  },
  'bite': {
    frames: 7,
    location: []
  },
  'ko': {
    frames: 12,
    location: []
  },
  'getHit': {
    frames: 4,
    location: []
  }
}

for (const [index, [key, value]] of Object.entries(Object.entries(animationStates))) {
  for (let framesIndex = 0; framesIndex < value.frames; framesIndex++) {
    let positionX = framesIndex * spriteWidth
    let postiionY = index * spriteHeigth
    value.location.push({x: positionX, y: postiionY})
  }
}

onMounted(() => {
  Init()
  AnimationLoop()
})

// function
const Init = () => {
  canvas = document.getElementById('canvas')
  ctx = canvas.getContext('2d')
  CANVAS_WIDTH = canvas.width = 600
  CANVAS_HEIGHT = canvas.height = 600

  playerImage.src = shadowDog
}

const AnimationLoop = () => {
  ctx.clearRect(0, 0, CANVAS_WIDTH, CANVAS_HEIGHT)
  // ctx.drawImage(image, sourceX, sourceY, sourceW, sourceH, destinationX, destinationY, destinationW, destinationH)
  let position = Math.floor(gameFrame / staggerFrames) % animationStates[spritStatus].location.length
  frameX = spriteWidth * position
  frameY = animationStates[spritStatus].location[position].y

  ctx.drawImage(playerImage, frameX , frameY, spriteWidth, spriteHeigth, 0, 0, spriteWidth, spriteHeigth)

  if (gameFrame > 10000) gameFrame = 0
  gameFrame++
  requestAnimationFrame(AnimationLoop)
}

</script>

<template lang="pug">
#ImageAnimation
  label(for="select")
  select(id="select" v-model="spritStatus")
    option(v-for="(key, value) in animationStates" :key="value") {{ `${value}` }}
  div(class="sprite")
    canvas(id="canvas")
</template>

<style lang="scss" scoped>
#ImageAnimation {
  & .sprite {
    position: relative;
    height: 100vh;
    width: 100vw;
    & #canvas {
      border: 4px solid black;
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 600px;
      height: 600px;
    }
  }
}
</style>
