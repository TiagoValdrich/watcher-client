<template>
  <div>
    <div class="input-group mb-3">
      <input
        type="text"
        class="form-control"
        placeholder="Insert your rtsp address"
        v-model="streamUrl"
      />
      <div class="input-group-append">
        <button v-on:click="playStream()" class="btn btn-success" type="button">
          <strong>Start watching</strong>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import environment from "../environment.js";

export default {
  data: () => {
    return {
      streamUrl: "",
    };
  },
  methods: {
    playStream() {
      axios
        .post(environment.API_URL + "/stream-url", {
          rtsp: this.streamUrl,
        })
        .then((response) => {
          if (response && response.data) {
            console.log("resposta", response);
            this.$emit("play-stream", response.data.stream_url);
          }
        })
        .catch((err) => {
          console.error("Error fetching stream URL", err);
        });
    },
  },
};
</script>

<style>
</style>