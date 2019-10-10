<template>
  <div id="timer" v-cloak>
    <div>
      <div class="timer">{{ prettyTime | prettify }}</div>
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
      let seconds = data[1];
      if (minutes < 10) {
        minutes = "0" + minutes;
      }
      if (seconds < 10) {
        seconds = "0" + seconds;
      }
      return minutes + ":" + seconds;
    }
  },
  data() {
    return {
      isRunning: false,
      pomodoroDuration: 5,
      time: 5,
      timer: null,
      sound: new Audio(
        "http://s1download-universal-soundbank.com/wav/nudge.wav"
      )
    };
  },
  computed: {
    prettyTime() {
      let time = this.time / 60;
      let minutes = parseInt(time);
      let seconds = Math.round((time - minutes) * 60);
      return minutes + ":" + seconds;
    }
  },
  methods: {
    start() {
      //only add pomodoro to database if timer is starting from 25 minutes
      if (this.time == this.pomodoroDuration) {
        this.$emit("add-Pomodoro");
      }

      this.isRunning = true;
      if (!this.timer) {
        this.timer = setInterval(() => {
          if (this.time > 0) {
            this.time--;
          } else {
            this.$emit("Change-Status-To-Finished");
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
      // this.$emit("Change-Status-To-Finished");
      this.stop();
      this.time = this.pomodoroDuration;
    },
    setTime(payload) {
      this.time = payload.minutes * 60 + payload.seconds;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
