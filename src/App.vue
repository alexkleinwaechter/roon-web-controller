<template lang="html">
  <v-app>
    <!-- <v-app-bar app>
    </v-app-bar> -->

    <!-- Sizes your content based upon application components -->
    <v-main>
      <!-- Provides the application the proper gutter -->
      <v-container fluid>
      <SvgSpriteMediaControlsCircle
        v-if="settings.general.use_circle_icons === true"
      />
      <SvgSpriteMediaControlsDefault v-else />
      <SvgSpriteMiscIcons />
      <AppDesktopNotifications />
      <AppBrowserTitle />        
        <router-view
          v-if="sw.paired && sw.paired === true"
          class="app_content"
        />
        <p v-else>
          The {{ sw.name }} extension is not enabled. Please use an official
          Roon client to enable it.
        </p>
      </v-container>
    </v-main>

    <v-footer app>
      <!-- -->
    </v-footer>
  </v-app>
</template>

<script>
import SvgSpriteMediaControlsDefault from "@/components/SvgSpriteMediaControlsDefault.vue";
import SvgSpriteMediaControlsCircle from "@/components/SvgSpriteMediaControlsCircle.vue";
import SvgSpriteMiscIcons from "@/components/SvgSpriteMiscIcons.vue";
import AppDesktopNotifications from "@/components/AppDesktopNotifications.vue";
import AppBrowserTitle from "@/components/AppBrowserTitle.vue";

export default {
  name: "app",
  components: {
    SvgSpriteMediaControlsDefault,
    SvgSpriteMediaControlsCircle,
    SvgSpriteMiscIcons,
    AppDesktopNotifications,
    AppBrowserTitle,
  },
  computed: {
    sw: {
      get() {
        return this.$store.state.socket.sw;
      },
    },
    settings: {
      get() {
        return this.$store.state.settings;
      },
    },
  },
};
</script>

