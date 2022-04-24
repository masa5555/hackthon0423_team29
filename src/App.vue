<template>
  <div id="app">
    <p>{{time}}</p>
    <button @click="recalc()">開始</button>
    <br />
    <br />
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
      ADD_UNIT: 10,
      time: `制限時間 0:30`
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
      const time_limit=30;
      this.time=`残り 0:30`;
      const start_time=new Date();
      const finish_time=start_time.getTime()+time_limit*1000;
      const self=this;

      while(new Date()-start_time<500);
      self.time=`残り 0:29`;

      let timerId=setInterval(function(){

        const timer=self.countdown(finish_time);
        self.time=`残り ${timer[0]}:${timer[1]}`;
        if (timer[1]==0) {
          self.time=`終了`;
          clearInterval(timerId);
        }
        else if(timer[1]<10){
          self.time=`残り ${timer[0]}:0${timer[1]}`;
        }

      }, 1000);
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
