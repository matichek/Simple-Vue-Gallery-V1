<template>

<!--This is Albums by User-->

<div>

    <v-container>

    <v-row>
      <v-col v-for="(album, i) in MergedUsersAlbumUrlsArray" :key="i" ref="UsersRef" :class="i" xl="2" lg="3" md="4" sm="6">
        <v-card ref="userRef" :data-key="album.id">

          <NuxtLink :to="'/albums/album/' + album.id"><v-img gradient="to bottom right, rgba(100,115,201,.33), rgba(25,32,72,.7)" :src="album.random_url[0]"></v-img></NuxtLink>

          <v-card-title class="gal-capitalize">
            {{album.title}}
          </v-card-title>
          <v-card-subtitle>

          </v-card-subtitle>

          <v-card-actions>
            <v-btn
              :to="'/albums/album/' + album.id"
              color="orange lighten-2"
              text
            >
              View Photos
          </v-btn>

          <v-spacer></v-spacer>
        </v-card-actions>

        </v-card>
    </v-col>

    </v-row>

      <!--Error displaying handling for API errors-->
      <v-snackbar
            v-model="errorAPI"
            :timeout="timeoutAPI"
          >
            {{ errorText }}
      </v-snackbar>

    </v-container>

</div>

</template>

<script>
import random from 'lodash/random'

export default {

    // Nuxt method for data fetching - asyncData

  async asyncData({ $axios, params }) {

    const res = await $axios.get(`https://jsonplaceholder.typicode.com/users/${params.albums}/albums`)
    const albumsByIdData = res.data


    const photosRes = await $axios.get('https://jsonplaceholder.typicode.com/photos')
    const photosResData = photosRes.data

    return { photosResData, albumsByIdData }
  },

  data() {
    return {
      MergedUsersAlbumUrlsArray: [],
      albumsByIdData: [],
      randomPicture: [],
      errorText: "Error",
      timeoutAPI: 1000,
      errorAPI: false
    }
  },

  async created() {

    await this.PrepPhotosArray()
    await this.PrepNewArrayForAlbums()

  },

  methods: {
    PrepPhotosArray() {
      return new Promise((resolve, reject) => {
        this.photosResData = this.photosResData.map(function (item) {
          return {
            albumId: item.albumId,
            id: item.id,
            url: item.url
          }
        })

        if (!this.error) {
          resolve()
        } else {
          reject(this.errorText)
          this.errorAPI = true
        }
      })
    },

    PrepNewArrayForAlbums() {
      return new Promise((resolve, reject) => {
        this.MergedUsersAlbumUrlsArray = this.albumsByIdData.map((obj) => {
          return {
            userId: obj.userId,
            id: obj.id,
            title: obj.title,
            p_id: this.getListOfPicturesBy(obj.id),
            random_id: this.getRandomValueFromArray(this.getListOfPicturesBy(obj.id)),
            random_url: this.getUrlBasedOnPictureId(obj.id)

          }
        })

        if (!this.error) {
          resolve()
        } else {
          reject(this.errorText)
          this.errorAPI = true
        }
      })
    },
    getListOfPicturesBy(getId) {
      // Getting specific object filtered by Id
      const albumListArray = this.photosResData.filter((e) => e.albumId === getId)

      // Retaining only the Ids of the albums
      const albumListArray2 = albumListArray.map(function (item) {
        return item.id
      })

      return albumListArray2
    },

      // Get random value from array
    getRandomValueFromArray(arr) {
      // Random value between first item in the array and last item
      const randomA = random(arr[0], arr[arr.length - 1])

      // Assined another variable to picking random urls in line 183
      this.randomPicture = randomA

      return randomA
    },

    // Get url based on id of pa picture
    getUrlBasedOnPictureId(getId) {

      const picListArray = this.photosResData.filter((e) => e.id === getId)

      // Retaining only the Ids of the albums
      const picListArray2 = picListArray.map(function (item) {
        return item.url
      })

      return picListArray2

    }

  }

}
</script>
