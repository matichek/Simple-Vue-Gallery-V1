/* https://jsonplaceholder.typicode.com/users/ */
/* https://jsonplaceholder.typicode.com/albums */

<template>
<v-container>

  <h1 class="mb-5">Users: ({{users.length}})</h1>

  <v-row>
    <v-col
      v-for="(user,i) in users" :key="i"
      xl="2"
      lg="3"
      md="4"
      sm="6"
    >
      <v-card
        class="mx-auto"
        max-width="344"
      >
      testF
      <p>Image id: {{testF(user['id'])}} {{res2}}</p>
      <p>Image id: {{testF(user['id'])}} {{res2}}</p>

      getImage
      <p>Image id: {{getImage(user['id'])}}</p>
      <p>Image id: {{getImage(1)}}</p>



      <v-img height="200"></v-img>

        <v-card-title>
          {{user.name}}
        </v-card-title>

        <v-card-subtitle>
          {{user.username}} <br>

          <!--{{Math.random(RelatedIdsUserAlbum(user['id'])[0].id, RelatedIdsUserAlbum(user['id']).length-1)}}-->
          <!--{{randomInteger(RelatedIdsUserAlbum(user['id'])[0].id, RelatedIdsUserAlbum(user['id']).length-1)}}-->
          <!--{{randomNumberOfAlbum(RelatedIdsUserAlbum(user['id'])[0].id, RelatedIdsUserAlbum(user['id']).length-1)}}-->

          <br>First album: {{RelatedIdsUserAlbum(user['id'])[0].id}}
          <br>Last album: {{RelatedIdsUserAlbum(user['id'])[0].id + RelatedIdsUserAlbum(user['id']).length-1}}

          <!--<li v-for="(album, j) in RelatedIdsUserAlbum(user['id'])" :key="j">
            {{album.id}}
          </li>-->
        </v-card-subtitle>

        <v-card-actions>
          <v-btn
            :to="'/albums?userId=' + user.id"
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




  <h3>Get first url from https://jsonplaceholder.typicode.com/albums/1/photos</h3>

  <h3>Test 3</h3>

</v-container>
</template>

<script>

import _ from 'lodash';

export default {

  async asyncData({ $axios, error }) {

      try {
          const usersData = await $axios.get('https://jsonplaceholder.typicode.com/users');
          const users = usersData.data;

          const albumData = await $axios.get('https://jsonplaceholder.typicode.com/albums');
          const albums = albumData.data;

          // const photoData = await $axios.get('https://jsonplaceholder.typicode.com/photos', {params: { id: 1 }});
          // const photos = photoData.data;


          // https://jsonplaceholder.typicode.com/albums/1/photos

          // const userId = 1;
          // const dataAlbum = await $axios.get('https://jsonplaceholder.typicode.com/albums/'+userId+'/photos');
          // const album = dataAlbum.data;
          // const albumPictureUrl = album[0].url;

          return {users, albums};

    } catch(e) {

        // If API is down
        error({
          statusCode: 503,
          errorAPI: true,
          messageError: 'Unable to fetch API.'
        })
      }

  },


  data() {
    return {
      usersNumber: 0,
      usersData: [],
      userId: "",
      users: [],
      randomAlbumdId: null,
      statusCode: null,
      messageErrorText: "Unable to fetch API. Try a bit later. Thank you ;)",
      errorAPI: false,
      res: [],
      res2: []
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
  },

  computed: {

    RelatedIdsUserAlbum() {
      return id => this.albums.filter(album => album.userId === id)
    },
    randomNumberOfAlbum(par1, par2) {
      // return _.this.random(1, par)
      return id => _.random(par1,par2)
    },
    randomInteger(min, max) {
      return id => this.Math.floor(Math.random() * (max - min + 1)) + min;
    }



  }, methods: {


    // How to get return value from promise ??? Testing

    getImage(albumId) {
      this.$axios
        .get('https://jsonplaceholder.typicode.com/albums/'+ albumId +'/photos')
        .then((res) => {

          const res2 = res.data

          return res2 + "test"
          }
        ).then(function(res2) {
          return "test22"
        })

      },

      // const test3 = test2.url;
      // return id => {
      //  return this.$axios.get('https://jsonplaceholder.typicode.com/albums/'+id+'/photos').data.url[0];
      // }

       /*  this.photos = this.$axios.get('https://jsonplaceholder.typicode.com/albums/'+id+'/photos');
        this.photosUrl = this.photos.data.url;
        return this.photosUrl; */
      // return "test"


    async dummyFetch(id) {

    },

    async testF(albumId) {

      const res = await this.$axios.get('https://jsonplaceholder.typicode.com/albums/'+ albumId +'/photos')



      return res

    }

  }

}
</script>
