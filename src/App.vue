<template>
  <div id="app">
    <calc />
  </div>
</template>

<script>
import calc from "./components/calc.vue";

export default {
  name: "App",
  components: {
    calc,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  height: 100%;
}
</style>
