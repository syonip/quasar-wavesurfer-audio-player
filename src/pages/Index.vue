<template>
  <div id="waveform"></div>
</template>

<script>
import WaveSurfer from "wavesurfer.js";
import { EventBus } from "../services/event-bus.js";

export default {
  name: 'PageIndex',
  data: () => ({
    wavesurfer: null,
  }),
  async mounted() {
    if (!this.wavesurfer) this.createWaveSurfer();

    EventBus.$on("fileChosen", file => {
      this.loadFile(file);
    });
  },
  methods: {
    createWaveSurfer() {
      this.wavesurfer = WaveSurfer.create({
        container: "#waveform",
        barWidth: 3
      });
    },
    loadFile(file) {
      if (file.target.files.length == 0) return;

      this.wavesurfer.loadBlob(file.target.files[0]);
    }
  }
}
</script>
