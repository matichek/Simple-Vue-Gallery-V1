<template>
<div>
<v-container>

<h1>Testing 3</h1>

<ul>

<li v-for="(user, i) in usersArrayPrepared" :key="i">
  {{user.user_name}} (UserId: {{user.userId}})

  <ul>

  <li v-for="(album, j) in RelatedIdsUserAlbum(user['userId'])" :key="j" style="display: inline-block" >
    <strong>Album: {{album['title']}}</strong>

    Pictures test:<br>
    {{picturesArray}}



  </li>

    <!--<br>First album: {{RelatedIdsUserAlbum(user['userId'])[0].userId}} -->

  </ul>


</li>

</ul>

<h1>Albums for 1</h1>
{{asyncAlbums}}

<h1>Albums:</h1>
{{albumsArrayPrepared}}

<h2>Users:</h2>
{{usersArrayPrepared}}

</v-container>

</div>
</template>

<script>


export default {

  data() {
    return {
      usersArrayPrepared: [],
      albumsArrayPrepared: [],
      asyncAlbums: [],
      rangeAlbumId: [],
      albumLength: null,
      randomAlbumNumber: null,
      picturesArray: []

    }
  }, computed: {

      RelatedIdsUserAlbum() {
        return userId => this.asyncAlbums.filter(album => album.userId === userId)
      },

      getAlbumLength() {

        this.albumsArrayPrepared.forEach(e => {

          this.rangeAlbumId.push(e.albumId)

        });

        return this.albumLength

        // return albumLength = this.asyncAlbums.length
      }

  },

  // Async getting all the API calls

  async created() {

    await this.getUsers()
    await this.getAlbums()

  }, updated() {

  },

  mounted() {



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
          .catch((e) => {
              this.errorAPI = true
              this.errorTextAPI = e
          })
    },

    // loadAsyncData() {

    //   this.usersArrayPrepared.map(async (user, i) => {
    //     const res = await asyncFunction()
    //   })

    // },


    // Get Photos from API into array

    // getAlbums() {

    //   // https://jsonplaceholder.typicode.com/users/1/albums

    // return this.$axios

    //   .get('https://jsonplaceholder.typicode.com/users/' + this.user.userId + '/albums')
    //   // .get('https://jsonplaceholder.typicode.com/users/albums')
    //   .then((res) => {

    //     // Load all data into object array
    //     this.albumsArray = res.data

    //     // Prepare object for merge with pictures object. Reduce size of array.
    //     this.albumsArrayPrepared = this.albumsArray.map(function(item) {
    //       return {
    //         albumId: item.id,
    //         title: item.title,
    //         userId: item.userId
    //       }
    //     });

    //     this.asyncAlbums = this.albumsArrayPrepared

    //   })
    //   .catch((e) => {

    //   })
    // }

    // https://jsonplaceholder.typicode.com/albums/1/photos

    async getAlbums() {

      // const res = await this.$axios.get('https://jsonplaceholder.typicode.com/users/' + this.userId + '/albums')
      const res = await this.$axios.get('https://jsonplaceholder.typicode.com/albums/')

      this.albumsArray = res.data

        // Prepare object for merge with pictures object. Reduce size of array.

        this.albumsArrayPrepared = this.albumsArray.map(function(item) {
          return {
            albumId: item.id,
            title: item.title,
            userId: item.userId
          }
        });

      this.asyncAlbums = res.data



    },

        // https://jsonplaceholder.typicode.com/albums/1/photos

    async getPhotos() {

      // const res = await this.$axios.get('https://jsonplaceholder.typicode.com/users/' + this.userId + '/albums')
      const res = await this.$axios.get('https://jsonplaceholder.typicode.com/albums/{params}/photos', {
        params: {
          param: this.$ref.user.userID
        }
      })

      this.picturesArray = res.data

        // Prepare object for merge with pictures object. Reduce size of array.

        // this.albumsArrayPrepared = this.albumsArray.map(function(item) {
        //   return {
        //     albumId: item.id,
        //     title: item.title,
        //     userId: item.userId
        //   }
        // });

      // this.asyncAlbums = res.data



    }

  }

}


</script>
