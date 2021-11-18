<template>

<!--This is Albums by User-->

<div>
  <v-container>

    <h3>Photos for Gallery ID {{$route.params.album}}</h3>

    <client-only>
      <vue-picture-swipe :items="PrepredPhotos"></vue-picture-swipe>
    </client-only>
  </v-container>

</div>

</template>

<script>


// Importing modal gallery solution

import Vue from 'vue'
import VuePictureSwipe from 'vue-picture-swipe'
Vue.use(VuePictureSwipe)

export default {

  components: {

    VuePictureSwipe

  },

  // Nuxt method for data fetching - asyncData

  async asyncData({ $axios, params }) {

    // https://jsonplaceholder.typicode.com/albums/1/photos

    const res = await $axios.get(`https://jsonplaceholder.typicode.com/albums/${params.album}/photos`)
    const photosByIdData = res.data
    // const albumId = res.data

    return { photosByIdData }
  },

  data() {
    return {
      alertFired: null,
      photosByIdData: [],
      PrepedPhotos: [],

    }
  },

  head() {
    return {
      title: 'Photos by Album',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Simple gallery with Vue/Nuxt/Vuetify. WIP 0_1'
        }
      ]
    }
  },

  async created() {

    await this.PrepPhotos()

  },

  methods: {

    // Preping photos obejct array to fit into the gallery required format (src, thumbnail, w, h, 'alt' as title)

    PrepPhotos() {
      return new Promise((resolve) => {

        this.PrepredPhotos = this.photosByIdData.map(function(item) {

          return {
            src: item.url,
            thumbnail: item.thumbnailUrl,
            w: 600,
            h: 600,
            alt: item.title

          }
        })

        resolve()

      })

    }

  }


}
</script>
