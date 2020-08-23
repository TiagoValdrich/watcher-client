<template>
  <div id="app">
    <div class="container">
      <div class="app-title">
        <h1>Watcher</h1>
        <small class="subtitle text-muted">Watching your RTSP stream has never been so easy... 🚀</small>
      </div>
      <div class="app-body mx-auto">
        <RtspInput v-on:play-stream="streamUrl = $event"></RtspInput>
        <transition name="fade">
          <div class="player" v-if="showPlayer">
            <Player :options="playerOptions" :streamUrl="streamUrl"></Player>
          </div>
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
import RtspInput from "./components/RtspInput";
import Player from "./components/Player";

export default {
  name: "App",
  components: {
    RtspInput,
    Player,
  },
  data() {
    return {
      showPlayer: false,
      streamUrl: "",
      playerOptions: {
        autoplay: true,
        controls: true,
        width: 960,
        height: 540,
        html5: {
          vhs: {
            withCredentials: false,
          },
        },
      },
    };
  },
  watch: {
    streamUrl(value) {
      if (typeof value == "string" && value.length > 7) {
        this.showPlayer = true;
      } else {
        this.showPlayer = false;
      }
    },
  },
};
</script>

<style>
body {
  background-color: #faf8f2;
}

@font-face {
  font-family: Lobster;
  src: url("./assets/Lobster-Regular.ttf");
}

.app-title {
  margin-top: 120px;
  margin-bottom: 60px;
  text-align: center;
  font-family: Lobster;
}

.app-body {
  text-align: center;
}

.app-body .player {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.subtitle {
  font-size: 16px;
}

.fade-player-enter-active,
.fade-player-leave-active {
  transition: opacity 0.5s;
}
.fade-player-enter,
.fade-player-leave-to {
  opacity: 0;
}
</style>
