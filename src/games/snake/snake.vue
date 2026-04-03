<template>
  <main>
    <div class="score">0</div>
    <div class="score">Total Score</div>
    <div class="stage"></div>
  </main>
</template>
<script setup lang="js">
import { onMounted, onUnmounted, ref } from "vue"
import g from "./useSnakeLogic.js"
let animFrameId
const score = ref(0)

onMounted(() => {
  g.config = {
    title: 'sssss',
    debug: window.location.hash == '#debug' ? 1 : 0,
    state: 'play'
  };
  g.setState(g.config.state)

  g.time = new g.Time()
  g.step = function() {
    animFrameId = requestAnimationFrame(g.step)
    g.states[g.state].step()
    g.time.update()
  }
  g.step()
})

onUnmounted(() => {
  g.states['play'].exit()  // this already cleans up keydown/resize listeners
  cancelAnimationFrame(animFrameId)
})
</script>
<style scoped>

* {
    box-sizing: border-box;
  }

  html,
  body {
    background-color: #000;
    height: 100%;
  }

  body {
    background: #222;
    background: radial-gradient(#333, #111);
    background-position: center center;
    background-repeat: no-repeat;
    background-size: cover;
    color: #fff;
    font: 100%/1.5 sans-serif;
    overflow: hidden;
  }

  .score {
    color: rgba(255, 255, 255, 0.5);
    font-size: 16px;
    font-weight: bold;
    padding-top: 5px;
    text-align: center;
  }

  /*================================================

  Stage

  ================================================*/

  .stage {
    bottom: 0;
    left: 0;
    margin: auto;
    position: absolute;
    right: 0;
    top: 0;
    z-index: 2;
  }

  /*================================================

  Tiles

  ================================================*/

  .tile {
    background: rgba(0, 0, 0, 0.15);
    position: absolute;
    transition-property:
      background,
      box-shadow,
      opacity,
      transform
    ;
    transform: translateZ(0);
    transition-duration: 3000ms;
  }

  .tile:before {
    bottom: 0;
    content: '';
    height: 0;
    left: 0;
    margin: auto;
    opacity: 0;
    position: absolute;
    right: 0;
    top: 0;
    width: 0;
    transition: opacity 300ms;
  }

  .tile.path:before {
    opacity: 1;
  }

  .tile.up:before {
    border-bottom: 4px inset rgba(255, 255, 255, 0.15);
    border-left: 4px solid transparent;
    border-right: 4px solid transparent;
  }

  .tile.down:before {
    border-top: 4px inset rgba(255, 255, 255, 0.15);
    border-left: 4px solid transparent;
    border-right: 4px solid transparent;
  }

  .tile.left:before {
    border-right: 4px inset rgba(255, 255, 255, 0.15);
    border-top: 4px solid transparent;
    border-bottom: 4px solid transparent;
  }

  .tile.right:before {
    border-left: 4px inset rgba(255, 255, 255, 0.15);
    border-top: 4px solid transparent;
    border-bottom: 4px solid transparent;
  }

  @media (max-width: 900px), (max-height: 900px) {
    .tile.up:before,
    .tile.down:before,
    .tile.left:before,
    .tile.right:before {
      border-width: 3px;
    }
  }

  @media (max-width: 500px), (max-height: 500px) {
    .tile.up:before,
    .tile.down:before,
    .tile.left:before,
    .tile.right:before {
      border-width: 2px;
    }
  }

  .tile.pressed {
    background: rgba(0, 0, 0, 0.3);
    box-shadow: inset 0 0 10px rgba(0, 0, 0, 0.6);
    transition-duration: 0ms;
  }
</style>