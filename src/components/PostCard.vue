<template>
  <q-card class="post-card q-mb-md" flat bordered>
    <q-item>
      <q-item-section avatar>
        <q-avatar>
          <img src="https://cdn.quasar.dev/img/boy-avatar.png" />
        </q-avatar>
      </q-item-section>

      <q-item-section>
        <q-item-label class="text-bold">priaajisn_</q-item-label>
        <q-item-label caption> {{post.location}} </q-item-label>
      </q-item-section>
    </q-item>

    <q-separator />

    <q-img :src='post.imgUrl' basic />

    <q-card-section>
      <div>{{post.caption}}</div>
      <div class="text-caption text-grey">{{post.date | formatedDate}}</div>
    </q-card-section>
  </q-card>
</template>

<script>
import { date } from 'quasar'
export default {
  name: "PostCard",
  props: {
      post: {
          type: Object,
          required: true
      }
  },
  filters : {
      formatedDate(value){
          return  date.formatDate(value, 'MMMM D h:mmA')
      }
  }
};
</script>

<style>
</style>