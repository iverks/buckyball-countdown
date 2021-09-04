<template>
  <div v-if="counting" class="clock-wrapper">
    <div class="hours">{{hours}}</div>
    <div class="dots">:</div> 
    <div class="minutes">{{minutes}}</div>
    <div class="dots">:</div> 
    <div class="seconds">{{seconds}}</div>
  </div>
  <div v-else>
    <h1>BUCKYBALL FUCKERS</h1>
  </div>
</template>

<script>
import { reactive, toRefs } from 'vue'

export default {
  setup () {
    const state = reactive({
      hours: 9,
      minutes: 51,
      seconds: 0,
      counting: true,
    })
    // Months are 0-indexed lol
    const nowperm = new Date;
    const then = new Date(nowperm.getFullYear(), 8, 10, 21, 51); 
    //const then = new Date(nowperm.getFullYear(), 8, 4, 17, 39);
    {
      if(then<nowperm) {
      then.setFullYear(nowperm.getFullYear()+1);
      }
    }
    const setTime = () => {
      var interval = setInterval(update, 1000)
      function update() {
        const now = new Date;
        const difftime = then-now;
        state.hours = addZero(Math.floor(difftime / (1000*60*60)));
        state.minutes = addZero(Math.floor((difftime - state.hours*1000*60*60) / (1000*60)));
        state.seconds = addZero(Math.floor((difftime - state.hours*1000*60*60 - state.minutes*1000*60) / (1000)));
        if(Math.floor(difftime/1000)==0) {
          clearInterval(interval);
          state.counting = false;
        }
      }
    }

    function addZero(num) {
      if(Math.abs(num)<10) {
        num = "0"+num;
      }
      return num;
    }
  
    return {
      ...toRefs(state),
      setTime,
      then
    }
  },
  mounted () {
    this.setTime(this.then)
  }
}
</script>

<style lang="css" scoped>
.hours, .minutes, .seconds {
  font-size: 8em;
  display: inline-flex;
}
.dots {
  font-size: 8em;
}
.clock-wrapper {
  display: block;
  justify-content: center;
  align-items: center;
  height: 100%;
  width: 100%;
  overflow: hidden;
}

</style> 