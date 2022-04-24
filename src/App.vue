<template>
  <div id="app">
    <!--{{CreateRandomColor()}}-->
    お題
    <div :style="{background:CallRandom,height:'50px',width:'50px',padding:'100px'}"></div>
    <button
      @mousedown="mouseDown1()"
      @mouseup="mouseUp()"
    >追加する1(赤)</button>
    <button
      @mousedown="mouseDown2()"
      @mouseup="mouseUp()"
    >追加する2(緑)</button>
    <button
      @mousedown="mouseDown3()"
      @mouseup="mouseUp()"
    >追加する3(青)</button>
    <button
      @click="resetGlass()"
    >リセット</button>

    <div class="glass_outer">
      <div
        class="glass_inner"
        :style="{
          background: computeCallrgb,
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
const RED = {
  r: 255,
  g: 0,
  b: 0
}
const GREEN = {
  r: 0,
  g: 255,
  b: 0
}
const BLUE = {
  r: 0,
  g: 0,
  b: 255
}

export default {
  name: 'App',
  components: {
    // HelloWorld
  },
  data() {
    return {
      glass: 0,
      mouseDownInterval: null,
      count1: 0,
      count2: 0,
      count3: 0,
      //random: 1
    }
  },
  computed:{
    computedGlassHeight(){
      return this.glass * ADD_UNIT
    },
    computedLiquidTop() {
      return GLASS_LIMIT - this.computedGlassHeight
    },
    add_color(){
      const color1 = RED
      const color2 = GREEN
      const color3 = BLUE
      return {
        r: (color1.r * this.count1 + color2.r * this.count2 + color3.r * this.count3) / (this.count1 + this.count2 + this.count3),
        g: (color1.g * this.count1 + color2.g * this.count2 + color3.g * this.count3) / (this.count1 + this.count2 + this.count3),
        b: (color1.b * this.count1 + color2.b * this.count2 + color3.b * this.count3) / (this.count1 + this.count2 + this.count3)
      }
    },
    computeCallrgb(){
      return this.rgb_to_css(this.add_color)
      },
    CallRandom(){
      return this.rgb_to_css(this.CreateRandomColor())
      }
    },
  methods: {
    resetMouseDownTime() {
      this.mouseDownTime = 0
    },
    glassHasSpaceValidate() {
      return this.glass < GLASS_LIMIT
    },
    mouseDown1() {
      this.resetMouseDownTime()
      this.mouseDownInterval = setInterval(
        () => {
          if (this.glassHasSpaceValidate()) {
            this.addGlass1()
          }
        },
        ADD_UNIT
      )
    },
    mouseDown2() {
      this.resetMouseDownTime()
      this.mouseDownInterval = setInterval(
        () => {
          if (this.glassHasSpaceValidate()) {
            this.addGlass2()
          }
        },
        ADD_UNIT
      )
    },
    mouseDown3() {
      this.resetMouseDownTime()
      this.mouseDownInterval = setInterval(
        () => {
          if (this.glassHasSpaceValidate()) {
            this.addGlass3()
          }
        },
        ADD_UNIT
      )
    },
    mouseUp() {
      clearInterval(this.mouseDownInterval)
    },
    resetGlass() {
      this.glass = 0
    },
    addGlass() {
      this.glass += 1
    },
    addGlass1() {
      this.addGlass()
      this.count1 += 1
    },
    addGlass2() {
      this.addGlass()
      this.count2 += 1
    },
    addGlass3() {
      this.addGlass()
      this.count3 += 1
    },
    rgb_to_css (color) {
      const r = (parseInt(color.r)).toString(16).padStart(2, '0')
      const g = (parseInt(color.g)).toString(16).padStart(2, '0')
      const b = (parseInt(color.b)).toString(16).padStart(2, '0')
      return `#${r}${g}${b}`
    },
    //CreateRandom(){
      //this.random = Math.floor(Math.random() * 256)
      //return this.random
    //}
    CreateRandomColor(){
      return{
        r : Math.floor(Math.random() * 256),
        g : Math.floor(Math.random() * 256),
        b : Math.floor(Math.random() * 256)
      }
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