<template>

<!--This is Albums by User-->


<div>

  <h3>Photos for Gallery ID {{$route.params.album}}</h3>

  <v-row class="gal-photos-row">
    <v-col
      v-for="(photo, i) in photosByIdData" :key="i"
      class="d-flex child-flex"
      cols="4"
      xl="3" lg="3" md="4"
      sm="12"
      xs="12"
    >
    <client-only>
      <vue-picture-swipe :items="[{src: 'https://picsum.photos/200/300', thumbnail: 'https://picsum.photos/200/300', w: 200, h: 300, alt: 'random picture', title: 'Some Picture'}, {src: 'https://picsum.photos/400/300', thumbnail: 'https://picsum.photos/400/300', w: 400, h: 300, alt: 'Another Random Picture', title: 'Another Picture'}]"></vue-picture-swipe>
    </client-only>
    <!--
      <v-img
        :src="photo.url"
        :lazy-src="photo.thumbnailUrl"
        aspect-ratio="1"
        :alt="photo.title"
        :class="getImageClass(i)"

        @click="onImageClick(i)"
      >
      -->
       <!-- <client-only placeholder="loading...">
          <expandable-image
            :src="photo.url"
          />
        </client-only>
 -->
        <template #placeholder>
          <v-row
            class="fill-height ma-0"
            align="center"
            justify="center"
          >
            <v-progress-circular
              indeterminate
              color="grey lighten-5"
            ></v-progress-circular>
          </v-row>
        </template>

      <!-- </v-img> -->


      <div class="gal-v-img-overlayed-title">{{photo.title}}</div>
    </v-col>
  </v-row>



</div>

</template>

<script>

import Vue from 'vue'
import VuePictureSwipe from 'vue-picture-swipe'
Vue.use(VuePictureSwipe)

// import Vue from 'vue'
// import VueExpandableImage from 'vue-expandable-image'
// Vue.use(VueExpandableImage)

export default {

  components: {

    VuePictureSwipe

  },

      // Nuxt method for data fetching - asyncData

  async asyncData({ $axios, params }) {

    // https://jsonplaceholder.typicode.com/albums/1/photos

    const res = await $axios.get(`https://jsonplaceholder.typicode.com/albums/${params.album}/photos`)
    const photosByIdData = res.data

    return { photosByIdData }
  },

  data() {
    return {
      alertFired: null,
      photosByIdData: []
    }
  },


}
</script>
