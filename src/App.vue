<template>
  <div id="app">
    <button @click="addGlass1()">追加する1</button>
      <div class="glass_outer">
        <div class="glass_inner" :style="{background:computeCallrgb, height: `${computedGlassHeight}px`, 'top': `${computedLiquidTop}px`}"></div>
      </div>
    <button @click="addGlass2()">追加する2</button>
    {{computeCallrgb}}
    {{computedGlassHeight}}
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
export default {
  name: 'App',
  components: {
    // HelloWorld
  },
  data() {
    return {
      glass: 0,
      count1: 0,
      count2: 0,
      ADD_UNIT: 10,
      color1: {
        r: 255,
        g: 0,
        b: 0
      },

      color2: {
        r: 0,
        g: 255,
        b: 0
      }
    }
  },
  computed:{
    computedGlassHeight(){
      return this.glass * this.ADD_UNIT
    },
    computedLiquidTop() {
      return 200 - this.computedGlassHeight
    },
    add_color(){
      return {
    r: (this.color1.r * this.count1 + this.color2.r * this.count2) / (this.count1 + this.count2),
    g: (this.color1.g * this.count1 + this.color2.g * this.count2) / (this.count1 + this.count2),
    b: (this.color1.b * this.count1 + this.color2.b * this.count2) / (this.count1 + this.count2)
      }
    },
    computeCallrgb(){
      return this.rgb_to_css(this.add_color)
    }
  },
  methods: {
    addGlass1() {
      this.glass = this.glass + 1
      this.count1 += 1
    },
    addGlass2() {
      this.glass = this.glass + 1
      this.count2 += 1
    },
    rgb_to_css (color) {
	const r = (parseInt(color.r)).toString(16).padStart(2, '0')
  const g = (parseInt(color.g)).toString(16).padStart(2, '0')
  const b = (parseInt(color.b)).toString(16).padStart(2, '0')
  return `#${r}${g}${b}`
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