<template>
  <q-page class="constrain-more q-pa-md">
    <div class="camera-frame q-pa-md">
      <img
        class="full-width"
        src="https://cdn.quasar.dev/img/parallax2.jpg"
        alt="camera-frame"
      />
    </div>

    <div class="text-center q-pa-md">
      <q-btn round color="grey-10" size="lg" icon="eva-camera" />

      <div class="row justify-center q-ma-md">
        <q-input class="col" label="Caption" dense />
      </div>

      <div class="row justify-center q-ma-md">
        <q-input class="col" label="Location" dense>
          <template v-slot:append>
            <q-btn round dense flat icon="my_location" />
          </template>
        </q-input>
      </div>

      <div class="row justify-center q-mt-lg">
        <q-btn unelevated rounded color="primary" label="Post Image" />
      </div>
    </div>
  </q-page>
</template>

<script>
import {uid} from 'quasar'
export default {
  name: "CameraPage",
  data(){
    return{
      post:{
        id : uid(),
        caption : '',
        location: '',
        date: Date.now(),
        imgURL: null
      }
    }
  }
};
</script>

<style lang="sass" scoped>
.camera-frame
    border : 2px solid $green-10
    border-radius : 10px
</style>
