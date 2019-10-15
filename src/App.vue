<template>
  <v-app>
    <v-app-bar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>POMO</span>
        <span class="font-weight-light">DORO</span>
      </v-toolbar-title>
    </v-app-bar>

    <v-content>
      <Pomodoro />
    </v-content>
  </v-app>
</template>

<script>
import Pomodoro from "./components/Pomodoro";

export default {
  name: "App",
  components: {
    Pomodoro
  },
  data: () => ({
    //
  })
};
</script>

<style lang="scss" scoped></style>
