<template>
  <q-layout view="lHh Lpr lFf">
    <q-header class="bg-white text-grey-10" bordered>
      <q-toolbar class="constrain">
        <q-btn
          class="large-screen-only q-mr-sm"
          flat
          round
          icon="eva-camera-outline"
          dense
          size="18px"
          to="/camera"
        />

        <q-separator class="large-screen-only" vertical spaced />

        <q-toolbar-title class="text-instagram text-bold">
          Quasargram
        </q-toolbar-title>
        <q-btn
          class="large-screen-only"
          flat
          round
          icon="eva-home-outline"
          dense
          size="18px"
          to="/"
        />
      </q-toolbar>
    </q-header>

    <q-footer class="bg-white" bordered>
      <div class="banner-container bg-primary">
        <div class="constrain">
          <q-banner
            v-if="showAppInstallBanner"
            inline-actions
            dense
            class="bg-primary text-white text-sm"
          >
            <template v-slot:avatar>
              <q-avatar
                color="white"
                text-color="grey-10"
                icon="eva-camera-outline"
                font-size="22px"
              />
            </template>
            <b>Install Quasargram ?</b>

            <template v-slot:action>
              <q-btn
                flat
                label="Yes"
                dense
                class="q-px-sm"
                @click="installApp"
              />
              <q-btn flat label="Later" dense class="q-px-sm" />
              <q-btn flat label="Never" dense class="q-px-sm" />
            </template>
          </q-banner>
        </div>
      </div>

      <q-tabs class="text-grey-10 small-screen-only" active-color="primary">
        <q-route-tab to="/" icon="eva-home-outline" />
        <q-route-tab to="/camera" icon="eva-camera-outline" />
      </q-tabs>
    </q-footer>
    <q-page-container class="bg-grey-3">
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
let deferredPrompt;
export default {
  name: "MainLayout",
  data() {
    return {
      showAppInstallBanner: false,
    };
  },
  methods: {
    installApp() {
      // Hide the app provided install promotion
      this.showAppInstallBanner = false;
      // Show the install prompt
      deferredPrompt.prompt();
      // Wait for the user to respond to the prompt
      deferredPrompt.userChoice.then((choiceResult) => {
        if (choiceResult.outcome === "accepted") {
          console.log("User accepted the install prompt");
        } else {
          console.log("User dismissed the install prompt");
        }
      });
    },
  },
  mounted() {
    window.addEventListener("beforeinstallprompt", (e) => {
      // Prevent the mini-infobar from appearing on mobile
      e.preventDefault();
      // Stash the event so it can be triggered later.
      deferredPrompt = e;
      // Update UI notify the user they can install the PWA
      this.showAppInstallBanner = true;
    });
  },
};
</script>

<style lang="sass">
.q-toolbar
  @media (min-width: $breakpoint-sm-min)
    height: 67px
.q-toolbar__title
  font-size: 28px
  @media (max-width: $breakpoint-xs-max)
    text-align: center
.q-footer
  .q-tab__icon
    font-size: 28px
</style>
