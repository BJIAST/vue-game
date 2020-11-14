<template>
  <div id="dashboard" class="dashboard">
   <div class="dashboard__left">
     <div class="dashboard__counter">
       {{ moves }} moves
     </div>
     <div class="dashboard__reset-wrap">
       <button @click="emitReset">Reset</button>
     </div>
   </div>
    <div class="dashboard__timer">
      Time:
      <span>{{ hours }}</span>
      <span>{{ minutes }}</span>
      <span>{{ seconds }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "Dashboard",
  props: ['gameStatus', 'moves'],
  data() {
    return {
      hours: '00',
      minutes: '00',
      seconds: '00',
      timer: null
    }
  },
  watch: {
    gameStatus(val) {
      if (val) {
        this.timer = setInterval(this.startTimer, 1000);
      } else {
        clearInterval(this.timer);
        this.hours = '00';
        this.minutes = '00';
        this.seconds = '00';
      }
    }
  },
  methods: {
    startTimer() {
      let seconds = parseInt(this.seconds) < 9 ? '0' + (parseInt(this.seconds) + 1) : parseInt(this.seconds) + 1;
      let minutes = this.minutes;
      let hours = this.hours;

      if (seconds === 60) {
        seconds = '00';
        minutes = parseInt(minutes) < 9 ? '0' + (parseInt(minutes) + 1) : parseInt(minutes) + 1;
      }

      if (minutes === 60) {
        minutes = '00';
        hours = parseInt(hours) < 9 ? '0' + (parseInt(hours) + 1) : parseInt(hours) + 1;
      }

      if (hours === 24) {
        hours = "00";
      }

      this.seconds = seconds;
      this.minutes = minutes;
      this.hours = hours;

    },
    emitReset(){
      this.$emit('resetGame');
    }
  }

}
</script>

<style scoped>

.dashboard {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 30px;
}

.dashboard__left {
  display: flex;
  align-items: center;
}
.dashboard__left button,
.dashboard__counter {
  background-color: #442a8f;
  color: #fff;
  border-radius: 4px;
  padding: 0 10px;
  line-height: 30px;
  font-weight: 400;
  margin-right: 20px;
  outline: none;
  border: 0;
  cursor: pointer;
}

.dashboard__left button {
  background-color: #100640;
}


.dashboard__timer > span {
  display: inline-flex;
  justify-content: center;
  height: 30px;
  width: 30px;
  line-height: 30px;
  background: #000;
  color: #fff;
  margin: 0 8px;
  position: relative;
}


.dashboard__timer > span:not(:last-child):after {
  content: ':';
  line-height: 30px;
  color: #000;
  font-size: 20px;
  position: absolute;
  right: -8px;
  top: 50%;
  transform: translate(50%, -50%);
  font-weight: 900;
}
</style>