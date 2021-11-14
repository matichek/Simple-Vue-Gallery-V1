<template>
<div>
<v-container>

<h1>Sitemap</h1>

<ul>

  <li v-for="(user, i) in usersArrayPrepared" :key="i">
    {{user.user_name}}
    <ul>
      <li style="display: inline-block" v-for="(album, j) in RelatedIdsUserAlbum(user['userId'])" :key="j">
        <strong>Album: {{album['album_title']}}</strong>

        <ul style="list-style: none">
          <li style="float: left;"  v-for="(photo, k) in RelatedPhotosAlbum(user['userId'])" :key="k">
            <v-img max-width="100" :src="photo['thumb']"></v-img>
          </li>
        </ul>
      </li>
    </ul>
  </li>

</ul>


<!-- Testing snackbar - notice for api finishing -->

<v-snackbar
      v-model="snackbar"
      :timeout="timeout"
    >
      {{ snackbarText }}

      <template v-slot:action="{ attrs }">
        <v-btn
          color="blue"
          text
          v-bind="attrs"
          @click="snackbar = false"
        >
          Close
        </v-btn>
    </template>
</v-snackbar>


<h1>Random number</h1>
{{randomNumber}}

<!--
<h1>Photos prepared</h1>
{{photosArrayPrepared}}

<h1>Album prepared</h1>
{{albumsArrayPrepared}}

<h1>Users Prepared</h1>
{{usersArrayPrepared}}
-->


</v-container>

</div>
</template>

<script>
import _ from 'lodash';

export default {

  data() {
    return {

      // Used arrays and vars

      usersArray: [],
      albumsArray: [],
      photosArray: [],
      albumsArrayPrepared: [],
      usersArrayPrepared: [],
      photosArrayPrepared: [],
      randomNumber: null,
      snackbar: false,
      snackbarText: "Photo API loaded.",
      timeout: 500
    }
  }, computed: {

      // Connect both userID from two different arrays

      RelatedIdsUserAlbum() {
        return id => this.albumsArrayPrepared.filter(album => album.userId === id)
      },

      RelatedPhotosAlbum() {
        return id => this.photosArrayPrepared.filter(album => album.albumId === id)
      }

  },

  mounted() {

  },

  // Async getting all the API calls

  async created() {

    await this.getUsers()
    await this.getAlbums()
    await this.getPhotos()


  }, updated() {

  },

  methods: {

    // First API call for getting user objects

    getUsers() {

        return this.$axios
          .get('https://jsonplaceholder.typicode.com/users')
          .then((res) => {
            this.usersArray = res.data

            // Prepare users object. We will use only id and name, username

            this.usersArrayPrepared = this.usersArray.map(function(item) {
              return {
                userId: item.id,
                user_name: item.name,
                username: item.username
              }
            })

          })
    },

    // Second API call for Albums

    getAlbums() {

    return this.$axios
      .get('https://jsonplaceholder.typicode.com/albums')
      .then((res) => {

        // Load all data into object array
        this.albumsArray = res.data

        // Prepare object for merge with users object. And lets just use id and title.
        this.albumsArrayPrepared = this.albumsArray.map(function(item) {
          return {
            userId: item.userId,
            album_id: item.id,
            album_title: item.title
          }

        });

        this.randomNumber = _.random(1,10)



      })
    },

    // Get Photos from API into array

    getPhotos() {

    return this.$axios

      .get('https://jsonplaceholder.typicode.com/photos')
      .then((res) => {

        // Load all data into object array
        this.photosArray = res.data

        // Prepare object for merge with pictures object. Reduce size of array.
        this.photosArrayPrepared = this.photosArray.map(function(item) {
          return {
            albumId: item.albumId,
            url: item.url,
            thumb: item.thumbnailUrl
          }



        });

        this.snackbar = true

      })
    }

  }

}


</script>
