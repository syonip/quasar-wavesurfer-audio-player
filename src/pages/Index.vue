<template>
  <div id="waveform"></div>
</template>

<script>
import WaveSurfer from "wavesurfer.js";

export default {
  name: 'PageIndex',
  data: () => ({
    wavesurfer: null,
  }),
  async mounted() {
    if (!this.wavesurfer) this.createWaveSurfer();
  },
  methods: {
    createWaveSurfer() {
      this.wavesurfer = WaveSurfer.create({
        container: "#waveform",
        barWidth: 3
      });
      this.wavesurfer.load(
        "https://ia902606.us.archive.org/35/items/shortpoetry_047_librivox/song_cjrg_teasdale_64kb.mp3"
      );
    }
  }
}
</script>
