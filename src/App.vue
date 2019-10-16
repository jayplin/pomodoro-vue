<template>
  <v-app>
    <v-app-bar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>POMO</span>
        <span class="font-weight-light">DORO</span>
      </v-toolbar-title>
    </v-app-bar>
    <v-content>
      <v-container fill-height>
        <v-layout text-center wrap align-center="true">
          <Pomodoro />
        </v-layout>
        <v-layout text-center wrap align-center="true" justify-end="true">
          <Table />
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import Pomodoro from "./components/Pomodoro";
import Table from "./components/Table";

export default {
  name: "App",
  components: {
    Pomodoro,
    Table
  },
  data: () => ({
    //
  })
};
</script>

<style lang="scss" scoped></style>
