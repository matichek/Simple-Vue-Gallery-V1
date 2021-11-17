<template>
<div>

<v-container>

<h1>Albums data id</h1>
{{albumsDataId}}

<h1>Random Array</h1>
<input v-model="arrayA" type="text">

<h1>Loops</h1>
{{MergedUsersAlbumUrlsArray}}

<h1>Loop prepared list</h1>

<v-row>

  <v-col v-for="(user, i) in preparedUsersData" :key="i" ref="UsersRef" :class="i" xl="2" lg="3" md="4" sm="6">
    <v-card ref="userRef" :data-key="user.userId">
      <v-card-title>
        {{user.name}} (ID: <span>{{user.userId}}</span>)
      </v-card-title>
      <v-card-subtitle>
        {{user.username}}<br>
      </v-card-subtitle>


    </v-card>
  </v-col>

</v-row>

<ul>
  <li v-for="(user,i) in MergedUsersAlbumUrlsArray" :key="i">
    {{user.name}}, {{user.username}}, {{user.randomAlbum}}
  </li>
</ul>


<h1>Prepared Users Data</h1>
{{preparedUsersData}}

<h1>Albums data</h1>
{{albumsData}}

<h1>Users Data</h1>
{{usersData}}

</v-container>

</div>

</template>

<script>

import _ from 'lodash';

// https://www.youtube.com/watch?v=PoRJizFvM7s

export default {

  // Nuxt method for data fetching - asyncData

  async asyncData({$axios}) {

    // Get users object
    const usersRes = await $axios.get('https://jsonplaceholder.typicode.com/users')
    const usersData = usersRes.data

    const albumsRes = await $axios.get('https://jsonplaceholder.typicode.com/albums')
    const albumsData = albumsRes.data

    return {usersData, albumsData}
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

      MergedUsersAlbumUrlsArray: [],
      UrlsByAlbumIdArray: [],

      errorText: "Error",
      errorAPI: false
    }
  },

  computed: {



  },

  // Async getting all the API calls

  async created() {

    await this.PrepUsersArray()
    await this.PrepNewArrayForUsers()
   // await this.getArrayOfUrlsFromAlbumId()

    // await this.getArrayOfUrlsFromAlbumId(2)
    // await this.getArrayOfUrlsFromAlbumId(3)


    // TODO Function that remove those big starter objects ??


  }, updated() {

  },

  mounted() {

  },

  methods: {

    // We are preparing new array with only the values we need for usersData api
    PrepUsersArray() {

      return new Promise((resolve, reject) => {
          this.preparedUsersData = this.usersData.map(function(item) {
            return {
              userId: item.id,
              name: item.name,
              username: item.username
            }
          })

          if(!this.error) {
            resolve()
          } else {
            reject(this.errorText)
            this.errorAPI = true
          }

      })
    },

    // Calculate albums function and prepare arrays

    PrepNewArrayForUsers() {
      return new Promise((resolve) => {

        // Get list of all albums

        // TODO Get list of all user ID and append list of all albums ID

        this.MergedUsersAlbumUrlsArray = this.preparedUsersData.map((obj) =>
          {
            return {
              "userId": obj.userId,
              "name": obj.name,
              "username": obj.username,
              "albumsList": this.getListOfAlbumsBy(obj.userId),
              "randomAlbum": this.getRandomValueFromArray(this.getListOfAlbumsBy(obj.userId)),

              // We need to populate this after this Prep is done
              "randomAlbumUrls": []

            }
          })

        // TODO get random Album number for each User
        // TODO get list of urls for that Random Album
        // this.albumTestNumber = this.usersData.find()

        resolve()

      })

    },

    // Filter and map results for nested array
    // TODO - reduce may be fast?

    getListOfAlbumsBy(getId) {

      // Getting specific object filtered by Id
      const albumListArray = this.albumsData.filter(e => e.userId === getId)

      // Retaining only the Ids of the albums
      const albumListArray2 = albumListArray.map(function(item) {return item.id})

      return albumListArray2
    },

    // Get random value from array
    getRandomValueFromArray(arr) {

      // Random value between first item in the array and last item
      const randomA = _.random(arr[0],arr[arr.length - 1])

      this.randomB = randomA

      return randomA
    },

    getArrayOfUrlsFromAlbumId() {

          return new Promise((resolve) => {


            const albumsRes = this.$axios.get('https://jsonplaceholder.typicode.com/album/1/photos')
            const albumsDataId = albumsRes.data

            // const albumsData2 = albumsData.map(function(e) { return e.url})

            this.MergedUsersAlbumUrlsArray[0].randomAlbumUrls.push(albumsDataId)



           resolve()

        }



      )}


    // https://jsonplaceholder.typicode.com/albums/1/photos
    // https://jsonplaceholder.typicode.com/albums/1/photos
  }
}

</script>
