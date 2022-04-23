<template>
  <div id="app">
    <button
      @mousedown="mouseDown()"
      @mouseup="mouseUp()"
    >追加する</button>
    <button
      @click="resetGlass()"
    >リセット</button>

    <div class="glass_outer">
      <div
        class="glass_inner"
        :style="{
          background:'lightblue',
          height: `${computedGlassHeight}px`,
          'top': `${computedLiquidTop}px`
        }"
      ></div>
    </div>

  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
const ADD_UNIT = 1
const GLASS_LIMIT = 200

export default {
  name: 'App',
  components: {
    // HelloWorld
  },
  data() {
    return {
      glass: 0,
      mouseDownInterval: null,
    }
  },
  computed:{
    computedGlassHeight(){
      return this.glass * ADD_UNIT
    },
    computedLiquidTop() {
      return GLASS_LIMIT - this.computedGlassHeight
    }
  },
  methods: {
    mouseDown() {
      this.mouseDownTime = 0
      this.mouseDownInterval = setInterval(
        () => {
          if (this.glass < GLASS_LIMIT) {
            this.glass += 1
          }
        },
        1
      )
    },
    mouseUp() {
      clearInterval(this.mouseDownInterval)
    },
    resetGlass() {
      this.glass = 0
    }
  }
}
</script>

<style>
.glass_outer{
  width: 100px;
  height: 200px;
  margin: 50px;
  border: 2px solid;
  border-top: none;
}

.glass_inner {
  width: 100px;
  position: relative;
}
</style>
