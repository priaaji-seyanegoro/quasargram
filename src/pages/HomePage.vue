<template>
  <q-page class="constrain q-pa-md">
    <div class="row row q-col-gutter-lg">
      <div class="col-12 col-sm-8">
        <template v-if="!isLoading && posts.length">
          <div v-for="post in posts" :key="post.id">
            <PostCard :post="post" />
          </div>
        </template>
        <template v-else-if="!isLoading && !posts.length">
          <h5 class="text-center text-grey">No Posts yet.</h5>
        </template>
        <template v-else>
          <SkeletonPostCard />
        </template>
      </div>
      <div class="col-4 large-screen-only">
        <q-item class="fixed">
          <q-item-section avatar>
            <q-avatar size="48px">
              <img src="https://cdn.quasar.dev/img/boy-avatar.png" />
            </q-avatar>
          </q-item-section>

          <q-item-section>
            <q-item-label class="text-bold">priaajisn_</q-item-label>
            <q-item-label caption> priaajisn_ </q-item-label>
          </q-item-section>
        </q-item>
      </div>
    </div>
  </q-page>
</template>

<script>
import { openDB } from "idb";
import PostCard from "../components/PostCard";
import SkeletonPostCard from "../components/SkeletonPostCard";
export default {
  name: "HomePage",
  components: {
    PostCard,
    SkeletonPostCard,
  },
  data() {
    return {
      posts: [],
      isLoading: false,
    };
  },
  computed: {
    serviceWorkerSupported() {
      if ("serviceWorker" in navigator) return true;
      return false;
    },
  },
  methods: {
    getPosts: async function () {
      this.isLoading = true;
      try {
        const response = await this.$axios.get(`${process.env.API}/posts`);
        this.posts = response.data;
        this.isLoading = false;
        if (!navigator.onLine) {
          this.getOfflinePost();
        }
      } catch (err) {
        this.$q.dialog({
          title: "Error",
          message: "Sorry something wrong, failed fetch data",
        });
        this.isLoading = false;
      }
    },
    getOfflinePost: async function () {
      try {
        const db = await openDB("workbox-background-sync");
        console.log("Database is open", db);
        const failedReqs = await db.getAll("requests");
        failedReqs.forEach((failReq) => {
          if (failReq.queueName === "createPostQueue") {
            const req = new Request(
              failReq.requestData.url,
              failReq.requestData
            );
            req.formData().then((formData) => {
              let offlinePost = {};
              offlinePost.id = formData.get("id");
              offlinePost.caption = formData.get("caption");
              offlinePost.location = formData.get("location");
              offlinePost.date = parseInt(formData.get("date"));
              offlinePost.offline = true;

              let reader = new FileReader();
              reader.readAsDataURL(formData.get("file"));
              reader.onloadend = () => {
                offlinePost.imgUrl = reader.result;
                this.posts.unshift(offlinePost);
              };
            });
          }
        });
      } catch (err) {
        console.log("ERR ACCESSING IDB", err);
      }
    },
    listenOfflinePostUploaded() {
      if (this.serviceWorkerSupported) {
        const channel = new BroadcastChannel("sw-messages");
        channel.addEventListener("message", (event) => {
          console.log("Received", event.data);
          if(event.data.msg === 'offline-post-uploaded'){
            let offlinePostCount = this.posts.filter(post => post.offline === true ).length
            this.posts[offlinePostCount - 1].offline = false;
          }
        });
      }
    },
  },
  activated(){
    this.getPosts();
  },
  created() {
    this.listenOfflinePostUploaded();
  },
};
</script>

<style lang="sass">
.post-card
  .badge-offline
    border-top-left-radius: 0
  .q-img
    min-height: 200px
</style>
