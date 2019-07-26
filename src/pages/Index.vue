
<template>
  <q-page class>
    <div class="audio-container">
      <div class="row q-ma-md">
        <div class="col-12">
          <div id="waveform"></div>
        </div>
      </div>
    </div>
    <div class="controls row flex flex-center fixed-bottom q-pb-md q-pt-md shadow-10">
      <div class>
        <q-btn
          color="primary"
          flat
          round
          icon="fast_rewind"
          size="xl"
          @click="wavesurfer.skipBackward(1)"
        />
        <q-circular-progress v-if="isLoading" size="72px" indeterminate color="primary" />
        <q-btn
          v-if="isPlaying"
          color="primary"
          round
          icon="pause"
          size="xl"
          @click="wavesurfer.playPause()"
        />
        <q-btn
          v-if="!isPlaying && !isLoading"
          color="primary"
          round
          icon="play_arrow"
          size="xl"
          @click="wavesurfer.playPause()"
        />
        <q-btn
          color="primary"
          flat
          round
          icon="fast_forward"
          size="xl"
          @click="wavesurfer.skipForward(1)"
        />
      </div>
    </div>
  </q-page>
</template>

<script>
import WaveSurfer from "wavesurfer.js";
import { EventBus } from "../services/event-bus.js";

export default {
  data: () => ({
    wavesurfer: null,
    isLoading: false
  }),
  async mounted() {
    EventBus.$on("fileChosen", file => {
      this.loadFile(file);
    });

    if (!this.wavesurfer) this.createWaveSurfer();
  },
  computed: {
    isPlaying() {
      if (!this.wavesurfer) return false;

      return this.wavesurfer.isPlaying();
    }
  },
  methods: {
    createWaveSurfer() {
      this.wavesurfer = WaveSurfer.create({
        container: "#waveform",
        barWidth: 3
      });

      this.wavesurfer.on("error", err => {
        console.error(err);
        this.isLoading = false;
        this.$q.notify({ message: err });
      });

      this.wavesurfer.on("loading", () => {
        this.isLoading = true;
      });

      this.wavesurfer.on("ready", () => {
        this.isLoading = false;
      });
    },
    loadFile(file) {
      if (file.target.files.length == 0) return;

      this.wavesurfer.loadBlob(file.target.files[0]);
    }
  },
  name: "PageIndex"
};
</script>
<style>

</style>
