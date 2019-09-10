<template>
  <div id="app">
    <TwitchBoard msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
import TwitchBoard from "./components/TwitchBoard.vue";

export default {
  name: "app",
  components: {
    TwitchBoard
  }
};
</script>

<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2a3a4a;
  background: #16A085;
  min-height: 100vh;
}
</style>
