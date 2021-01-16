<template>
  <q-page class="constrain-more q-pa-md">
    <div class="camera-frame q-pa-md">
      <video
        class="full-width"
        autoplay
        ref="video"
        v-show="!isCapturedImage"
      />
      <canvas
        ref="canvas"
        class="full-width"
        height="240"
        v-show="isCapturedImage"
      />
    </div>

    <div class="text-center q-pa-md">
      <q-btn
        v-if="hasCameraSupport"
        round
        color="grey-10"
        size="lg"
        icon="eva-camera"
        @click="captureImage"
      />

      <q-file
        v-model="isImageUpload"
        outlined
        v-else
        label="Choose an Image"
        accept="image/*"
        @input="captureImageFallback"
      >
        <template v-slot:prepend>
          <q-icon name="eva-attach-outline" />
        </template>
      </q-file>

      <div class="row justify-center q-ma-md">
        <q-input class="col" label="Caption" dense />
      </div>

      <div class="row justify-center q-ma-md">
        <q-input class="col" :loading="locationLoading" v-model="post.location" label="Location" dense>
          <template v-slot:append>
            <q-btn v-if="!locationLoading && locationSupported" round dense flat icon="my_location" @click="getLocation" />
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
import { uid } from "quasar";
require("md-gum-polyfill");
export default {
  name: "CameraPage",
  data() {
    return {
      post: {
        id: uid(),
        caption: "",
        location: "",
        date: Date.now(),
        imgURL: null,
      },
      isCapturedImage: false,
      isImageUpload: [],
      hasCameraSupport: true,
      locationLoading : false
    };
  },
  computed:{
    locationSupported(){
      if('geolocation' in navigator) return true
      return false
    }
  },
  methods: {
    initCamera() {
      //ask permission for access cam
      navigator.mediaDevices
        .getUserMedia({
          video: true,
        })
        .then((stream) => {
          this.$refs.video.srcObject = stream;
        })
        .catch((err) => {
          //user rejected to access camera
          this.hasCameraSupport = false;
        });
    },
    captureImageFallback(file) {
      //store img to state
      this.post.imgURL = file;

      //write canvas from upload image
      let canvas = this.$refs.canvas;
      let context = canvas.getContext("2d");
      var reader = new FileReader();
      reader.onload = (event) => {
        var img = new Image();
        img.onload = () => {
          canvas.width = img.width;
          canvas.height = img.height;
          context.drawImage(img, 0, 0);
          //show canvas
          this.isCapturedImage = true;
        };
        img.src = event.target.result;
      };
      reader.readAsDataURL(file);
    },
    captureImage() {
      //get dom
      let video = this.$refs.video;
      let canvas = this.$refs.canvas;
      // set canvas height and width
      canvas.width = video.getBoundingClientRect().width;
      canvas.height = video.getBoundingClientRect().height;
      // stream video to canvas
      let context = canvas.getContext("2d");
      context.drawImage(video, 0, 0, canvas.width, canvas.height);
      this.isCapturedImage = true;
      //convert file to blob
      this.post.imgURL = this.dataURItoBlob(canvas.toDataURL());
      this.disableCamera();
    },
    disableCamera() {
      this.$refs.video.srcObject.getVideoTracks().forEach((track) => {
        track.stop();
      });
    },
    dataURItoBlob(dataURI) {
      // convert base64 to raw binary data held in a string
      // doesn't handle URLEncoded DataURIs - see SO answer #6850276 for code that does this
      var byteString = atob(dataURI.split(",")[1]);

      // separate out the mime component
      var mimeString = dataURI.split(",")[0].split(":")[1].split(";")[0];

      // write the bytes of the string to an ArrayBuffer
      var ab = new ArrayBuffer(byteString.length);

      // create a view into the buffer
      var ia = new Uint8Array(ab);

      // set the bytes of the buffer to the correct values
      for (var i = 0; i < byteString.length; i++) {
        ia[i] = byteString.charCodeAt(i);
      }

      // write the ArrayBuffer to a blob, and you're done
      var blob = new Blob([ab], { type: mimeString });
      return blob;
    },
    getLocation() {
      this.locationLoading = true;
      navigator.geolocation.getCurrentPosition(
        (position) => {
          this.getCityAndCountry(position);
        },
        (err) => {
          this.locationError();
        },
        { timeout: 7000 }
      );
    },
    getCityAndCountry(position) {
      let apiUrl = `https://geocode.xyz/${position.coords.latitude},${position.coords.longitude}?json=1`;
      this.$axios
        .get(apiUrl)
        .then((res) => {
          this.locationSuccess(res);
        })
        .catch((err) => {
          this.locationError();
        });
    },
    locationSuccess(res) {
      this.post.location = res.data.city;
      if (res.data.country) {
        this.post.location += `, ${res.data.country}`;
      }
      this.locationLoading = false
    },
    locationError() {
      this.$q.dialog({
        title: "Error",
        message: "Sorry something wrong, cannot find your location",
      });
      this.locationLoading = false
    },
  },
  mounted() {
    this.initCamera();
  },
  beforeDestroy() {
    if (this.hasCameraSupport) {
      this.disableCamera();
    }
  },
};
</script>

<style lang="sass" scoped>
.camera-frame
    border : 2px solid $green-10
    border-radius : 10px
</style>
