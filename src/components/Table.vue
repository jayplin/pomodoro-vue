<template>
  <v-data-table
    :headers="headers"
    :items="reports"
    :items-per-page="5"
    class="elevation-1"
  ></v-data-table>
</template>

<script>
const axios = require("axios");

export default {
  data: function() {
    return {
      reports: "",
      headers: [
        { text: "ID", value: "pomodoro_id" },
        { text: "Timestamp", value: "timestamp" },
        { text: "Task", value: "task_name" }
      ]
    };
  },
  mounted() {
    this.getReports();
  },
  methods: {
    getReports() {
      axios
        .get("https://pomodoro-node-api.herokuapp.com/pomodoros")
        .then(response => {
          this.reports = response.data;
        });

      console.log(this.reports);
    }
  }
};
</script>
