/* https://jsonplaceholder.typicode.com/users/ */
/* https://jsonplaceholder.typicode.com/albums */

<template>
<v-container>

  <h1 class="mb-5">Users: ({{users.length}})</h1>

  <v-row>
    <v-col
      v-for="(user, index) in users" :key="index"
      xl="2"
      lg="3"
      md="4"
      sm="6"
    >
      <v-card
        class="mx-auto"
        max-width="344"
      >

        <v-img
          src="https://www.google.com"
          height="200px"
        ></v-img>


        <v-card-title>
          {{user.name}}
        </v-card-title>

        <v-card-subtitle>
          {{user.username}}
        <br>
          Album url test static:
        {{album[0].url}}

        <br>
          Album url test test:
        <strong>{{album[0].url}}</strong>
        <!--<strong>{{dataAlbumF(`${user.id}`)[0].url}}</strong>-->
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

  <div v-if="users.length">
    <span>{{users.length}}</span>
  </div>
  <div>
  {{users}}
  </div>

  <h2>Test2</h2>


  <h3>Get first url from https://jsonplaceholder.typicode.com/albums/1/photos</h3>
  {{album[0].url}}

  <h3>Dynmic function for albums?</h3>



</v-container>
</template>

<script>

/* Example of SEO info */

export default {

/*   let getall = async() => {

      const res = await axios.get(`https://jsonplaceholder.typicode.com/users`);
      const users = res.data;



      const res2 = await axios.get(`https://jsonplaceholder.typicode.com/photos?albumId`);

      const albums = res2.data;

      return albums, users
  }, */

  async asyncData({ $axios, error }) {

      try {
          const data = await $axios.get('https://jsonplaceholder.typicode.com/users');
          const users = data.data;

          const dataAlbum = await $axios.get(`https://jsonplaceholder.typicode.com/albums/1/photos`);
          const album = dataAlbum.data;

          return {users, album};

/*           return {
            users: data,
            album: dataAlbum
         } */

    } catch(e) {

        // If API is down
        error({
          statusCode: 503,
          message: 'Unable to fetch API. Try a bit later. Thank you ;)'
        })
      }

  },
  data() {
    return {
      usersNumber: 0,
      apiUrl: "https://jsonplaceholder.typicode.com",
      album2: []
    }
  },
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
  }
}

</script>
