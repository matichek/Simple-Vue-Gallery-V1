<template>

  <div>

    <v-container>

    <v-row>
      <v-col v-for="(user, i) in MergedUsersAlbumUrlsArray" :key="i" ref="UsersRef" :class="i" xl="2" lg="3" md="4" sm="6">
        <v-card ref="userRef" :data-key="user.userId">

          <a :href="'/albums/' + user.userId"><v-img gradient="to bottom right, rgba(100,115,201,.33), rgba(25,32,72,.7)" :src="user.randomAlbumUrls"></v-img></a>

          <v-card-title>
            {{user.name}}
          </v-card-title>
          <v-card-subtitle>
            {{user.username}}<br>
          </v-card-subtitle>

          <v-card-actions>
            <v-btn
              :to="'/albums/' + user.userId"
              color="orange lighten-2"
              text
            >
              View Albums
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
import _ from 'lodash'

// https://www.youtube.com/watch?v=PoRJizFvM7s

export default {


  // Nuxt method for data fetching - asyncData

  async asyncData({ $axios }) {
    // Get users object
    const usersRes = await $axios.get(
      'https://jsonplaceholder.typicode.com/users'
    )
    const usersData = usersRes.data

    const albumsRes = await $axios.get(
      'https://jsonplaceholder.typicode.com/albums'
    )
    const albumsData = albumsRes.data

    const photosRes = await $axios.get(
      'https://jsonplaceholder.typicode.com/photos'
    )
    const photosResData = photosRes.data

    return { usersData, albumsData, photosResData }
  },

  // Define all vars, arrays

  data() {
    return {
      usersData: [],
      preparedUsersData: [],
      albumData: [],
      albumsDataId: [],

      usersMerged: [],
      albumNumber: [],
      randomA: null,
      randomB: null,
      arrayA: [],
      randomAlbumUrls: null,

      MergedUsersAlbumUrlsArray: [],
      UrlsByAlbumIdArray: [],

      errorText: 'Error',
      errorAPI: false,
      timeoutAPI: 1000,
    }
  },

  /* Example of SEO info */

  head() {
    return {
      title: 'Vue.js Gallery with Axios/Nuxt/Vuetify',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Simple gallery with Vue/Nuxt/Vuetify. WIP 0_1'
        }
      ]
    }
  },

  computed: {},

  // Async getting all the API calls

  async created() {
    await this.PrepUsersArray()
    await this.PrepNewArrayForUsers()
    await this.PrepPhotosArray()

    // TODO Function that remove those big starter objects ??
  },
  updated() {},

  mounted() {},

  methods: {
    // We are preparing new array with only the values we need for usersData api
    PrepUsersArray() {
      return new Promise((resolve, reject) => {
        this.preparedUsersData = this.usersData.map(function (item) {
          return {
            userId: item.id,
            name: item.name,
            username: item.username,
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

    // Preparing final Object Array that has all info that we need for v-for loop.

    PrepNewArrayForUsers() {
      return new Promise((resolve, reject) => {
        this.MergedUsersAlbumUrlsArray = this.preparedUsersData.map((obj) => {
          return {
            userId: obj.userId,
            name: obj.name,
            username: obj.username,
            albumsList: this.getListOfAlbumsBy(obj.userId),
            randomAlbum: this.getRandomValueFromArray(
              this.getListOfAlbumsBy(obj.userId)
            ),


            // We need to populate this after this Prep is done - this can in improved cause we are getting the whole url object, which is not neccessary.
            // TODO - we have random number of the album - for optimization, we call only specific albums and get only those images
            randomAlbumUrls: this.getListOfUrlsByAlbumId(this.randomB),
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

    // We are preparing new array with only the values we need for usersData api
    PrepPhotosArray() {
      return new Promise((resolve, reject) => {
        this.photosResData = this.photosResData.map(function (item) {
          return {
            albumId: item.albumId,
            id: item.id,
            url: item.thumbnailUrl,
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

    // Filter and map results for nested array
    // TODO - reduce may be fast?

    getListOfAlbumsBy(getId) {
      // Getting specific object filtered by Id
      const albumListArray = this.albumsData.filter((e) => e.userId === getId)

      // Retaining only the Ids of the albums
      const albumListArray2 = albumListArray.map(function (item) {
        return item.id
      })

      return albumListArray2
    },

    // Get random value from array
    getRandomValueFromArray(arr) {
      // Random value between first item in the array and last item
      const randomA = _.random(arr[0], arr[arr.length - 1])

      // Assined another variable to picking random urls in line 183
      this.randomB = randomA

      return randomA
    },

    // List all of the urls for album ID
    getListOfUrlsByAlbumId(getAlbumId) {
      // Getting specific object filtered by Id
      const photoListArray = this.photosResData.filter(
        (e) => e.albumId === getAlbumId
      )

      // Retaining only the Ids of the albums
      const photoListArray2 = photoListArray.map(function (item) {
        return item.url
      })

      // Picking random url from the array of urls
      const randomUrl = _.random(photoListArray2)
      const randomUrl2 = photoListArray2[randomUrl]

      return randomUrl2
    },
  },
}
</script>
