<template lang="html">
  <v-btn icon v-on:click="go_command">
    <v-icon> {{ getIcon() }} </v-icon>
  </v-btn>
</template>

<script>
import { mdiPause } from "@mdi/js";
import { mdiSkipPrevious } from "@mdi/js";
import { mdiSkipNext } from "@mdi/js";
import { mdiPlay } from "@mdi/js";
import { mdiStop } from "@mdi/js";
export default {
  name: "ButtonPlayCommand",
  props: {
    command: { type: String, required: true },
  },
  data() {
    return {
      mdiPause: mdiPause,
      mdiSkipPrevious: mdiSkipPrevious,
      mdiSkipNext: mdiSkipNext,
      mdiPlay: mdiPlay,
      mdiStop: mdiStop,
    };
  },
  methods: {
    go_command: function () {
      let payload = {
        command: this.command,
      };
      this.$store.dispatch("GO_command", payload);
    },
    getIcon() {
      switch (this.command) {
        case "pause":
          return this.mdiPause;
        case "previous":
          return this.mdiSkipPrevious;
        case "next":
          return this.mdiSkipNext;
        case "play":
          return this.mdiPlay;
        case "stop":
          return this.mdiStop;
        default:
          break;
      }
    },
  },
};
</script>
