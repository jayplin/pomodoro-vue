<template>
  <div class="pom">
    <h1>{{ msg }}</h1>
    <p>Task: {{ task }}</p>
    <timer v-on:add-Pomodoro="addPomodoro" v-on:Change-Status-To-Finished="changeStatusToFinished"></timer>
    <input v-model="task" placeholder="type task name" />

    <button v-on:click="reset">Reset</button>
  </div>
</template>

<script>
const axios = require("axios");
import Timer from "./Timer.vue";
export default {
  name: "Pomodoro",
  components: {
    Timer
  },
  props: {
    msg: String
  },
  data: function() {
    return {
      task: "",
      finished: "false",
      timerStarted: "false"
    };
  },
  methods: {
    addPomodoro: function() {
      //post current task from input field
      axios
        .post("https://pomodoro-node-api.herokuapp.com/pomodoros", {
          timestamp: new Date().toISOString(),
          finished: this.finished,
          task_name: this.task
        })
        .then(function(response) {
          console.log(response);
        })
        .catch(function(error) {
          console.log(error);
        });

      //start timer
    },
    changeStatusToFinished: function() {
      this.finished = true;
      console.log(this.finished);
    },
    reset: function() {
      //set time to 0
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
