<template>
  <div class="underlay" />
  <div v-if="counting" class="clock-wrapper">
      <div class="clock-border">
      <div class="hours">{{hours}}</div>
      <div class="dots">:</div> 
      <div class="minutes">{{minutes}}</div>
      <div class="dots">:</div> 
      <div class="seconds">{{seconds}}</div>
    </div>
  </div>
  <div v-else>
    <Celebration />
  </div>
</template>

<script>
import { reactive, toRefs } from 'vue'
import Celebration from "./Celebration.vue"
export default {
  name: "Countdown",
  components: {
    Celebration
  },
  setup () {
    const state = reactive({
      hours: "00",
      minutes: "00",
      seconds: "00",
      counting: true,
    })
    // Months are 0-indexed lol
    const nowperm = new Date;
    const then = new Date(nowperm.getFullYear(), 8, 10, 21, 51); 
    //const then = new Date(nowperm.getFullYear(), 8, 4, 17, 39);
    {
      if(then<nowperm&&(then.getMonth()<nowperm.getMonth()||then.getDate()<nowperm.getDate())) {
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
        if(Math.floor(difftime/1000)==0 || (difftime<0 && now.getMonth()==then.getMonth() && now.getDate()==then.getDate())) {
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
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  width: 100%;
  margin: auto;
}

.clock-border {
  background-color: #399ad5;
  color: #ddd;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: auto;
  box-shadow: 0px 2px 20px -7px rgba(40, 40, 40, 0.9);
  border-radius: 2em;
  padding: 4px 8px 4px 4px;
}

.underlay {
  background-color:#ddd;
  position:fixed;
  width:100%;
  height:100%;
  top:0px;
  left:0px;
  z-index:-1000;
}
</style> 