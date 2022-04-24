<template>
  <div id="app" class="app">

    <div class="app-left">
      <p>{{time}}</p>
      <p>スコア：{{ computedCurrentScore }}</p>
      <button
        @click="recalc()"
        v-bind:disabled="active"
        class="button-start"
      >開始</button>
      <p>お題</p>
      <div
        v-if="target_color"
        :style="{
          background: CallRandom,
          height:'50px',
          width:'50px',
          padding:'50px'
        }"
      ></div>
    </div>

    <div class="app-right">
      <div class="bar-upper">
        <button
          @mousedown="mouseDown1()"
          @mouseup="mouseUp()"
          v-bind:disabled="!active"
          class="button-red"
        >赤を追加する</button>
        <button
          @mousedown="mouseDown2()"
          @mouseup="mouseUp()"
          v-bind:disabled="!active"
          class="button-green"
        >緑を追加する</button>
        <button
          @mousedown="mouseDown3()"
          @mouseup="mouseUp()"
          v-bind:disabled="!active"
          class="button-blue"
        >青を追加する</button>
        <button
          @mousedown="mouseDown4()"
          @mouseup="mouseUp()"
          v-bind:disabled="!active"
          class="button-reduce"
        >薄める</button>
        <button
          @click="resetGlass()"
          class="button-reset"
          v-bind:disabled="!active"
        >リセット</button>
      </div>
      <div class="bar-lower">
        <div class="bar-lower-left"></div>
        <div class="bar-lower-center">
          <div class="glass_outer">
            <div v-if="click1"><!--ボタンを押すと赤のジュースが出ます-->
              <div class="glass_drop" :style="{background:'#ff0000'}"></div>
            </div>
            <div v-if="click2"><!--ボタンを押すと緑のジュースが出ます-->
              <div class="glass_drop" :style="{background:'#00ff00'}"></div>
            </div>  
            <div v-if="click3"><!--ボタンを押すと青のジュースが出ます-->
              <div class="glass_drop" :style="{background:'#0000ff'}"></div>
            </div>
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
        <div class="bar-lower-right"></div>
      </div>
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
const WHITE = {
  r:255,
  g:255,
  b:255
}

export default {
  name: 'App',
  components: {
    // HelloWorld
  },
  data() {
    return {
      glass: 0,
      time: `制限時間 0:30`,
      active: false,
      mouseDownInterval: null,
      count1: 0,
      count2: 0,
      count3: 0,
      count4: 0,
      click1: false,
      click2: false,
      click3: false,
      target_color: null
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
      const color4 = WHITE
      return {
        r: (color1.r * this.count1 + color2.r * this.count2 + color3.r * this.count3 + color4.r * this.count4) / (this.count1 + this.count2 + this.count3 + this.count4),
        g: (color1.g * this.count1 + color2.g * this.count2 + color3.g * this.count3 + color4.g * this.count4) / (this.count1 + this.count2 + this.count3 + this.count4),
        b: (color1.b * this.count1 + color2.b * this.count2 + color3.b * this.count3 + color4.b * this.count4) / (this.count1 + this.count2 + this.count3 + this.count4)
      }
    },
    computeCallrgb(){
      return this.rgb_to_css(this.add_color)
      },
    CallRandom(){
      return this.rgb_to_css(this.target_color)
    },
    computedCurrentScore() {
      return this.ScoreCalculation()
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
      this.click1 = true
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
      this.click2 = true
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
      this.click3 = true
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
    mouseDown4() {
      this.resetMouseDownTime()
      this.mouseDownInterval = setInterval(
        () => {
          if (this.glassHasSpaceValidate()) {
            this.addGlass4()
          }
        },
        ADD_UNIT
      )
    },
    mouseUp() {
      this.click1 = false
      this.click2 = false
      this.click3 = false
      clearInterval(this.mouseDownInterval)
    },
    resetGlass() {
      this.glass = 0
      this.count1 = 0
      this.count2 = 0
      this.count3 = 0
      this.count4 = 0
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
      // 乱数初期化
      this.target_color = this.CreateRandomColor()

      const self=this;
      self.active=true;
      self.time=`残り 0:30`;
      const time_limit=30;
      const start_time=new Date();
      const finish_time=start_time.getTime()+time_limit*1000;

      while(new Date()-start_time<500);
      self.time=`残り 0:29`;

      let timerId = setInterval(
        function(){
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
        },
        1000
      );
    },
    addGlass() {
      this.glass += 0.1
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
    addGlass4() {
      this.addGlass()
      this.count4 += 1
    },
    rgb_to_css (color) {
      const r = (parseInt(color.r)).toString(16).padStart(2, '0')
      const g = (parseInt(color.g)).toString(16).padStart(2, '0')
      const b = (parseInt(color.b)).toString(16).padStart(2, '0')
      return `#${r}${g}${b}`
    },
    CreateRandomColor(){
      return {
        r : Math.floor(Math.random() * 256),
        g : Math.floor(Math.random() * 256),
        b : Math.floor(Math.random() * 256)
      }
    },
    ScoreCalculation(){
      if (!this.target_color) {
        return '-'
      }

      const score = Math.ceil(
        (
          (255 - Math.abs(this.target_color.r - this.add_color.r)) *
          (255 - Math.abs(this.target_color.g - this.add_color.g)) *
          (255 - Math.abs(this.target_color.b - this.add_color.b)) /
          (255 ** 3)
        ) * 100
      )
      return score
    }
  }
}
</script>

<style>

.app {
  display: flex;
  margin-top: 50px;
}
.app-left, .app-right {
  width: 45%;
}
.bar-upper {
  padding-top: 50px;
  display: flex;
  justify-content: space-around;
  width: 533px;
  height: 150px;
  background: grey;
  z-index: 100;
}
.bar-lower {
  display: flex;
}
.bar-lower-left, .bar-lower-right {
  width: 200px;
  height: 250px;
  background: grey;
}

.button-red {
  height: 50px;
  background: indianred;
}
.button-green {
  height: 50px;
  background: lightgreen;
}
.button-blue {
  height: 50px;
  background: lightblue;
}
.button-start {
  height: 50px;
  width: 100px;
}
.button-reset {
  height: 50px;
}
.button-reduce {
  height: 50px;
}

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
.glass_drop {
  position: absolute;
  width: 20px;
  top: 410px;
  height: 250px;
  margin: -160px 47px 0;
  opacity: 1;
}

</style>