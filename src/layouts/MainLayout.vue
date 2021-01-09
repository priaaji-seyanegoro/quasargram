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

    <q-footer class="bg-white small-screen-only" bordered>
      <q-tabs class="text-grey-10" active-color="primary">
        <q-route-tab to="/" icon="eva-home-outline" />
        <q-route-tab to="/camera" icon="eva-camera-outline" />
      </q-tabs>
    </q-footer>
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
export default {
  name: "MainLayout",
  data() {
    return {};
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
