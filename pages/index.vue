/* https://jsonplaceholder.typicode.com/users/ */
/* https://jsonplaceholder.typicode.com/albums */

<template>
<v-container>

  <h1 class="mb-5">Users: ({{users.length}})</h1>

  <v-row>
    <v-col
      v-for="user in users" :key="user.id"
      xl="2"
      lg="3"
      md="4"
      sm="6"
    >
      <v-card
        class="mx-auto"
        max-width="344"
      >
      <p></p>
      <v-img height="200"></v-img>

        <v-card-title>
          {{user.name}}
        </v-card-title>

        <v-card-subtitle>
          {{user.username}}
        </v-card-subtitle>

        <v-card-actions>
          <v-btn
            :to="'/albums?userId=' + user.id"
            color="orange lighten-2"
            text
          >
            View Album
          </v-btn>

          <v-spacer></v-spacer>
        </v-card-actions>

      </v-card>

    </v-col>
  </v-row>




  <h3>Get first url from https://jsonplaceholder.typicode.com/albums/1/photos</h3>

  <h3>Test 3</h3>

</v-container>
</template>

<script>

export default {

  async asyncData({ $axios, error }) {

      try {
          const data = await $axios.get('https://jsonplaceholder.typicode.com/users');
          const users = data.data;

          // const userId = 1;
          // const dataAlbum = await $axios.get('https://jsonplaceholder.typicode.com/albums/'+userId+'/photos');
          // const album = dataAlbum.data;
          // const albumPictureUrl = album[0].url;

          return {users};

    } catch(e) {

        // If API is down
        error({
          statusCode: 503,
          message: 'Unable to fetch API. Try a bit later. Thank you ;)'
        })
      }

  },

  // Idea - make anohter async asyncData2 function and make const

  data() {
    return {
      usersNumber: 0,
      userId: "",
      users: []
    }
  },

  /* Example of SEO info */

  head() {
    return {
      title: 'Matijas Gallery Test with Nuxt.js',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Simple gallery with Vue/Nuxt/Vuetify'
        }
      ]
    }
  }, method: {
    getImage(id) {

      const getImage2 = this.$axios.get('https://jsonplaceholder.typicode.com/albums/'+id+'/photos').data

      return getImage2

       /*  this.photos = this.$axios.get('https://jsonplaceholder.typicode.com/albums/'+id+'/photos');
        this.photosUrl = this.photos.data.url;
        return this.photosUrl; */

    }


  }

}
</script>
