<template>
  <div id="app">
    <v-app>
      <v-banner v-if="deferredPrompt" color="info" dark class="text-left">
        Get our free app. It won't take up space on your phone and also works
        offline!
        <template v-slot:actions>
          <v-btn text @click="dismiss">Dismiss</v-btn>
          <v-btn text @click="install">Install</v-btn>
        </template>
      </v-banner>
      <div class="pa-4 text-center">
        <img alt="Vue logo" src="./assets/logo.png" />
        <h1>Customize Your Vue.js PWA Installation</h1>
      </div>
    </v-app>
  </div>
</template>

<script>
import Cookies from "js-cookie";
export default {
  name: "App",
  data() {
    return {
      deferredPrompt: null
    };
  },
  created() {
    window.addEventListener("beforeinstallprompt", e => {
      e.preventDefault();
      // Stash the event so it can be triggered later.
      if (Cookies.get("add-to-home-screen") === undefined) {
        this.deferredPrompt = e;
      }
    });
    window.addEventListener("appinstalled", () => {
      this.deferredPrompt = null;
    });
  },
  methods: {
    async dismiss() {
      Cookies.set("add-to-home-screen", null, { expires: 15 });
      this.deferredPrompt = null;
    },
    async install() {
      this.deferredPrompt.prompt();
    }
  }
};
</script>
