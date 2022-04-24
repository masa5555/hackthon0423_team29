<template>
  <div id="app">
    <p>{{time}}</p>
    <button @click="recalc()" v-bind:disabled="active">開始</button>
    <br />
    <br />
    お題
    <div :style="{background:CallRandom,height:'50px',width:'50px',padding:'100px'}"></div>
    <button
      @mousedown="mouseDown1()"
      @mouseup="mouseUp()"
      v-bind:disabled="!active"
    >追加する1(赤)</button>
    <button
      @mousedown="mouseDown2()"
      @mouseup="mouseUp()"
      v-bind:disabled="!active"
    >追加する2(緑)</button>
    <button
      @mousedown="mouseDown3()"
      @mouseup="mouseUp()"
      v-bind:disabled="!active"
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
          top: `${computedLiquidTop}px`
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
      ADD_UNIT: 10,
      time: `制限時間 0:30`,
      active: false,
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
      this.count1 = 0
      this.count2 = 0
      this.count3 = 0
    },
    addGlass() {
      this.glass = this.glass + 1
    },
    countdown(finish_time){
      const now=new Date();
      const rest=finish_time-now.getTime();
      const sec=Math.floor(rest/1000)%60;
      const min=Math.floor(rest/1000/60)%60;
      const rest_time=[min, sec];
      return rest_time;
    },
    recalc(){
      const self=this;
      self.active=true;
      self.time=`残り 0:30`;
      const time_limit=30;
      const start_time=new Date();
      const finish_time=start_time.getTime()+time_limit*1000;

      while(new Date()-start_time<500);
      self.time=`残り 0:29`;

      let timerId=setInterval(function(){

        const timer=self.countdown(finish_time);
        self.time=`残り ${timer[0]}:${timer[1]}`;
        if (timer[1]==0) {
          self.time=`終了`;
          self.active=false;
          clearInterval(timerId);
        }
        else if(timer[1]<10){
          self.time=`残り ${timer[0]}:0${timer[1]}`;
        }

      }, 1000);
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