<template>
  <div id="timer" v-cloak>
    <h1>Timer</h1>
    <timer-setup v-if="!time" @set-time="setTime"></timer-setup>
    <div v-else>
      <timer :time="prettyTime"></timer>
      <div>
        <button v-if="!isRunning" @click="start">Start</button>
        <button v-if="isRunning" @click="stop">Stop</button>
        <button @click="reset">Reset</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Timer",
  filters: {
    prettify: function(value) {
      let data = value.split(":");
      let minutes = data[0];
      let secondes = data[1];
      if (minutes < 10) {
        minutes = "0" + minutes;
      }
      if (secondes < 10) {
        secondes = "0" + secondes;
      }
      return minutes + ":" + secondes;
    }
  },
  data: {
    isRunning: false,
    minutes: 0,
    secondes: 0,
    time: 0,
    timer: null,
    sound: new Audio("http://s1download-universal-soundbank.com/wav/nudge.wav")
  },
  computed: {
    prettyTime() {
      let time = this.time / 60;
      let minutes = parseInt(time);
      let secondes = Math.round((time - minutes) * 60);
      return minutes + ":" + secondes;
    }
  },
  methods: {
    start() {
      this.isRunning = true;
      if (!this.timer) {
        this.timer = setInterval(() => {
          if (this.time > 0) {
            this.time--;
          } else {
            clearInterval(this.timer);
            this.sound.play();
            this.reset();
          }
        }, 1000);
      }
    },
    stop() {
      this.isRunning = false;
      clearInterval(this.timer);
      this.timer = null;
    },
    reset() {
      this.stop();
      this.time = 0;
      this.secondes = 0;
      this.minutes = 0;
    },
    setTime(payload) {
      this.time = payload.minutes * 60 + payload.secondes;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
