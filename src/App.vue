<template>
  <div id="app">
    <button @click="addGlass()">追加する</button>
    <div class="glass_outer">
      <div class="glass_inner" :style="{background:'lightblue', height: `${computedGlassHeight}px`, 'top': `${computedLiquidTop}px`}"></div>
    </div>
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
      ADD_UNIT: 10
    }
  },
  computed:{
    computedGlassHeight(){
      return this.glass * this.ADD_UNIT
    },
    computedLiquidTop() {
      return 200 - this.computedGlassHeight
    }
  },
  methods: {
    addGlass() {
      this.glass = this.glass + 1
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
