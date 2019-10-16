<template>
  <div class="pom">
    <h1>{{ msg }}</h1>
    <p class="taskname">Task: {{ task_name }}</p>
    <timer v-on:add-Pomodoro="addPomodoro" v-on:Change-Status-To-Finished="changeStatusToFinished"></timer>
    <v-text-field v-model="task_name" placeholder="type task name"/>
  </div>
</template>

<script>
const axios = require("axios");
const shortid = require("shortid");

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
      pomodoro_id: "",
      task_name: "",
      finished: "false",
      timestamp: "",
      timerStarted: "false"
    };
  },
  methods: {
    addPomodoro: function() {
      //create vars
      (this.pomodoro_id = shortid.generate()),
        (this.finished = "false"),
        (this.timestamp = new Date().toISOString()),
        //post current task from input field
        axios
          .post("https://pomodoro-node-api.herokuapp.com/pomodoros", {
            pomodoro_id: this.pomodoro_id,
            timestamp: this.timestamp,
            finished: this.finished,
            task_name: this.task_name
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
      this.turnDeskOnAndOFF();

      //change on database
      axios
        .put(
          "https://pomodoro-node-api.herokuapp.com/pomodoros" +
            "/" +
            this.pomodoro_id,
          {
            finished: this.finished,
            timestamp: this.timestamp,
            task_name: this.task_name
          }
        )
        .then(function(response) {
          console.log(response);
        })
        .catch(function(error) {
          console.log(error);
        });

      console.log(this.pomodoro_id + " is finished: " + this.finished);

      // set all variables to zero
      this.pomodoro_id = "";
      this.timestamp = "";
      this.finished = false;
      this.task_name = "";
    },
    turnDeskOnAndOFF: async function() {
      axios.post(
        "http://openhabianpi:8080/rest/items/Licht_Schreibtisch",
        "OFF",
        {
          headers: { "Content-Type": "text/plain" }
        }
      );
      await new Promise(done => setTimeout(done, 2000));
      axios.post(
        "http://openhabianpi:8080/rest/items/Licht_Schreibtisch",
        "ON",
        {
          headers: { "Content-Type": "text/plain" }
        }
      );
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
p.taskname {
  font-size: 300%;
}
</style>
