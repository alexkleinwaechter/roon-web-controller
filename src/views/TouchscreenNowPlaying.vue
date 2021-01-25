<template lang="html">
  <v-card data-name="TouchscreenNowPlaying">
    <v-row
      v-if="current_zone && current_zone.now_playing"
      class="now_playing_page"
    >
      <v-col cols="12" class="library_view_button_container" style="border 2px solid red">
        <router-link
          tag="button"
          class="view_button"
          v-bind:to="{ name: 'TouchscreenLibrary' }"
        >
          <svg>
            <use href="#svg_chevron_up" />
          </svg>
        </router-link>
      </v-col>
      <v-col cols="6" class="cover_art_container">
        <ImageCover
          class="cover_art"
          v-if="current_zone.now_playing.image_key"
          v-bind:image_key="current_zone.now_playing.image_key"
        />
      </v-col>
      <v-col cols="6"  class="song_info_container">
        <div class="text_bold">
          {{
            current_zone.now_playing.three_line.line1.replace(/\ \/\ /g, ", ")
          }}
        </div>
        <div>
          {{
            current_zone.now_playing.three_line.line2.replace(/\ \/\ /g, ", ")
          }}
        </div>
        <div>
          {{
            current_zone.now_playing.three_line.line3.replace(/\ \/\ /g, ", ")
          }}
        </div>
      </v-col>
      <v-col cols="12"  class="control_container">
        <TouchscreenControlsMedia class="media_control_container" />
        <template v-if="current_zone.is_seek_allowed === true">
          <input
            class="range_track_seek"
            type="range"
            min="0"
            max="100"
            v-bind:value="
              Math.floor(
                (current_zone.now_playing.seek_position /
                  current_zone.now_playing.length) *
                  100
              )
            "
          />
          <div class="song_position_container">
            <div>
              {{ secondsToTime(current_zone.now_playing.seek_position) }}
            </div>
            <div v-if="show_time_remaining === true">
              {{
                secondsToTime(
                  current_zone.now_playing.length -
                    current_zone.now_playing.seek_position
                )
              }}
            </div>
            <div v-else>
              {{ secondsToTime(current_zone.now_playing.length) }}
            </div>
          </div>
        </template>
        <TouchscreenControlsOverlays class="overlay_controls" />
      </v-col>
      <v-col cols="12"  class="queue_view_button_container">
        <router-link
          tag="button"
          class="view_button"
          v-bind:to="{ name: 'TouchscreenQueue' }"
          ><svg>
            <use href="#svg_chevron_down" /></svg
        ></router-link>
      </v-col>
      <UiClock
        v-if="clock.show_clock"
        v-bind:class="'clock_' + clock.position"
      />
    </v-row>
    <v-row v-else class="not_playing_page">
      <v-col cols="12"  class="library_view_button_container">
        <router-link
          tag="button"
          class="view_button"
          v-bind:to="{ name: 'TouchscreenLibrary' }"
        >
          <svg>
            <use href="#svg_chevron_up" />
          </svg>
        </router-link>
      </v-col>
      <v-col cols="12" class="not_playing_info">
        Nothing Playing
      </v-col>
      <TouchscreenControlsOverlays class="not_playing_control_container" />
      <v-col cols="12" class="queue_view_button_container">
        <router-link
          tag="button"
          class="view_button"
          v-bind:to="{ name: 'TouchscreenQueue' }"
          ><svg>
            <use href="#svg_chevron_down" /></svg
        ></router-link>
      </v-col>
      <UiClock v-if="clock.show_clock" class="clock_not_playing" />
    </v-row>
  </v-card>
</template>

<script>
import TouchscreenControlsMedia from "@/components/TouchscreenControlsMedia.vue";
import TouchscreenControlsOverlays from "@/components/TouchscreenControlsOverlays.vue";

export default {
  name: "TouchscreenViewNowPlaying",
  components: {
    TouchscreenControlsMedia,
    TouchscreenControlsOverlays,
  },
  computed: {
    current_zone: {
      get() {
        return this.$store.state.roon.zone_list[
          this.$store.state.settings.current_zone_id
        ];
      },
    },
    clock: {
      get() {
        return this.$store.state.settings.clock;
      },
    },
    show_time_remaining: {
      get() {
        return this.$store.state.settings.general.show_time_remaining;
      },
    },
  },
};
</script>
