<template>
  <div id="app">
    <img alt="Title" src="./assets/title.png" />
    <!-- <div>
      <video-player :options="videoOptions">
        <source src="/media/examples/flower.mp4" type="video/mp4" />
      </video-player>
    </div> -->
    <LoLanePicker />
  </div>
</template>

<script>
import LoLanePicker from "./components/MainScreen.vue";
// import VideoPlayer from "@/components/VideoPlayer.vue";
// import "video.js/dist/video-js.css";

export default {
  name: "App",
  components: {
    // VideoPlayer,
    LoLanePicker,
  },
  // data() {
  //   return {
  //     videoOptions: {
  //       autoplay: true,
  //       controls: true,
  //       sources: [
  //         {
  //           src: "/assets/godfreeze.mp4",
  //           type: "video/mp4",
  //         },
  //       ],
  //     },
  //   };
  // },
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
