<template>
  <div>
    <video
      id="player"
      class="video-js vjs-big-play-centered"
      poster="../assets/poster.jpg"
      ref="videoPlayer"
    ></video>
  </div>
</template>

<script>
import videojs from "video.js";

export default {
  name: "VideoPlayer",
  props: {
    options: {
      type: Object,
      default() {
        return {};
      },
    },
    streamUrl: {
      type: String,
      default() {
        return "";
      },
    },
  },
  data() {
    return {
      player: null,
    };
  },
  mounted() {
    this.player = videojs(this.$refs.videoPlayer, this.options, () => {
      this.player.src({
        type: "application/x-mpegURL",
        src: this.streamUrl,
      });
    });

    this.player.on("error", () => {
      setTimeout(() => {
        console.log("Retry...");
        this.player.src({
          type: "application/x-mpegURL",
          src: this.streamUrl,
        });
      }, 5000);
    });
  },
  beforeDestroy() {
    if (this.player) {
      this.player.dispose();
    }
  },
};
</script>

<style>
</style>