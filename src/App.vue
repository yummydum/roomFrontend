<template>
  <div>
    <div id="app">
      <date-line-graph measure="temperature" />
    </div>
    <div id="app">
      <date-line-graph measure="humidity" />
    </div>
    <div id="app">
      <date-line-graph measure="cpu_temperature" />
    </div>
  </div>
</template>

<script>
import DateLineGraph from "./components/DateLineGraph.vue";

export default {
  name: "App",
  components: {
    DateLineGraph,
  },
};
</script>


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
