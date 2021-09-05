<template>
  <div id="app">
    <img alt="Title" src="./assets/title.png" />
    <LoLanePicker />
  </div>
</template>

<script>
import LoLanePicker from "./components/MainScreen.vue";

export default {
  name: "App",
  components: {
    LoLanePicker,
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
