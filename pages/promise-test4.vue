<template>
<div>



<v-container>
<h1>Loops</h1>
{{listOfAllUsers}}

<h1>Loop prepared list</h1>

<v-row>

  <v-col v-for="(user, i) in preparedUsersData" :key="`UsersRef-${i}`" ref="UsersRef" :class="i" xl="2" lg="3" md="4" sm="6">
    <v-card ref="userRef" :data-key="user.userId">
      <v-card-title>
        {{user.name}} (ID: <span ref="UserIdRef">{{user.userId}}</span>)
      </v-card-title>
      <v-card-subtitle>
        {{user.username}}<br>Random Album number<span ref="randomAlbumNumber">1</span>
      </v-card-subtitle>
      <span ref="randomAlbumNumber">1</span><br>
      Ref album num:<span>{{albumNumber}}</span><br>

      <span ref="refTest">
      Ref TEST: {{albumTestNumber}}
      </span>
    </v-card>
  </v-col>

</v-row>


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

// import _ from 'lodash';
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
      // preparedAlbumData: [],
      usersMerged: [],

      albumNumber: [],
      albumTestNumber: [],


      listOfAllUsers: [],



      errorText: "Error",
      errorAPI: false
    }
  },

  computed: {



  },

  // Async getting all the API calls - 3rd proper way of doing async stuff

  async created() {

    await this.prepareUsers()
    await this.getAlbumNumber()


  }, updated() {

  },

  mounted() {

  },

  methods: {

    // We are preparing new array with only the values we need for usersData api
    prepareUsers() {

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

    getAlbumNumber() {
      return new Promise((resolve) => {

        // Get list of all albums



        // TODO Get list of all user ID and append list of all albums ID

        this.listOfAllUsers = this.preparedUsersData.map((obj) =>
          {
            return {
              "userId": obj.userId,
              "albumsList": this.getListOfAlbumsBy(obj.userId)
            }
          })

        // TODO loop thourgh list and find list of all albums AND create an object push and push to array



        // TODO get random Album number for each User



        // TODO get list of urls for that Random Album



        // this.albumTestNumber = this.usersData.find()

        resolve()

        // this.refTest = this.refTest.push(this.$attrs.UsersRef)

        // Find specific object
        // this.albumNumber = this.albumsData.find(x => x.id === this.$refs.UsersRef[this.index])
        // this.albumNumber = this.albumData.filter(obj => {
        //   // return obj.id === this.$refs.UserIdRef
        //   return obj.id === 1
        // })

      })

    },

    // Filter and map results for nested array

    getListOfAlbumsBy(getId) {

      // Getting specific object filtered by Id
      const albumListArray = this.albumsData.filter(e => e.userId === getId)

      // Retaining only the Ids of the albums
      const albumListArray2 = albumListArray.map(function(item) {return item.id})

      return albumListArray2
    }



    // https://jsonplaceholder.typicode.com/albums/1/photos
    // https://jsonplaceholder.typicode.com/albums/1/photos
  }
}

</script>
