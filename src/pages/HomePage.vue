<template>
  <q-page class="constrain q-pa-md">
    <div class="row row q-col-gutter-lg">
      <div class="col-12 col-sm-8">
        <div v-for="post in posts" :key="post.id">
          <PostCard :post="post" />
        </div>
      </div>
      <div class="col-4">
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
import PostCard from "../components/PostCard";
export default {
  name: "HomePage",
  components: {
    PostCard,
  },
  data() {
    return {
      posts: [
        {
          id: 1,
          caption: "Dance all night",
          date: 1610156882008,
          location: "Zodiac, Jakarta",
          imgUrl:
            "https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fmanual.co.id%2Fwp-content%2Fuploads%2F2019%2F01%2F14-01-2019-Zodiac-Bar_Senopati-3-980x719.jpg&f=1&nofb=1",
        },
        {
          id: 2,
          caption: "Dance all night",
          date: 1610156882008,
          location: "Zodiac, Jakarta",
          imgUrl:
            "https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fmanual.co.id%2Fwp-content%2Fuploads%2F2019%2F01%2F14-01-2019-Zodiac-Bar_Senopati-3-980x719.jpg&f=1&nofb=1",
        },
        {
          id: 3,
          caption: "Dance all night",
          date: 1610156882008,
          location: "Zodiac, Jakarta",
          imgUrl:
            "https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fmanual.co.id%2Fwp-content%2Fuploads%2F2019%2F01%2F14-01-2019-Zodiac-Bar_Senopati-3-980x719.jpg&f=1&nofb=1",
        },
        {
          id: 4,
          caption: "Dance all night",
          date: 1610156882008,
          location: "Zodiac, Jakarta",
          imgUrl:
            "https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fmanual.co.id%2Fwp-content%2Fuploads%2F2019%2F01%2F14-01-2019-Zodiac-Bar_Senopati-3-980x719.jpg&f=1&nofb=1",
        },
      ],
    };
  },
};
</script>

<style lang="sass">
.post-card
  .q-img
      min-height : 200px
</style>
