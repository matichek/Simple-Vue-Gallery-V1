<template>
<div>

<v-container>
  <v-row>
    <v-col>
    <ul id="albumsList" ref="albumList">

    </ul>
  </v-col>
  </v-row>
</v-container>
<h2>Testing</h2>
{{usersData2}}

<h1>Users</h1>
{{usersData}}

<v-snackbar
      v-model="errorAPI"
      :timeout="timeoutAPI"
    >
      {{ errorText }}

</v-snackbar>

</div>



</template>

<script>

// import _ from 'lodash';
// https://www.youtube.com/watch?v=PoRJizFvM7s

export default {

    async asyncData( {$axios}) {

        const data = await $axios.get('https://jsonplaceholder.typicode.com/users');
        const usersData = data.data

        const data2 = await $axios.get('https://jsonplaceholder.typicode.com/albums');
        const albumsData = data2.data

      return {usersData, albumsData}
  },

  data() {
    return {

      timeout: 500,
      errorText: "Error API",
      generalError: null,
      errorAPI: false,

      arr: ['test', 'test2', 'test3'],
      newArr: [],
      data2: [],
      usersData: [],
      usersData2: [],
      albumsData: [],
      usersNumber: [],
      randomAlbum: [],
      albumsNumbers: [],
      listOfUserIds:  []
    }
  }, computed: {

  },

  // Async getting all the API calls - 3rd proper way of doing async stuff

  async created() {

    await this.createPost({name: "Matija", username: "Matichek"})

    this.getUsers()

    // this.getAlbumDataById()


  }, updated() {

  },

  mounted() {

    // One way of dealing with promisses
    // this.createPost({name: "Matija", username: "Matichek" })
    //   .then(this.getPosts)
    //   .catch(err => err.generalError)

    // Better way

    // const promise1 = Promise.resolve('Hello World');
    // const promise2 = Promise.resolve('Hello World');
    // const promise4 = fetch('https://jsonplaceholder.typicode.com/users/').then(res => res.json())

    // Promise.all([promise1, promise2, promise4]).then(values => this.usersData2)

  },

  methods: {

    getUsers() {

      setTimeout(() => {

        let output = '';

        this.usersData.forEach((user, index) => {
          output += `<v-card>${user.name}</v-card>`;
        });

        // The way to reference 'ID' with ref property in the element in the dom
        this.$refs.albumList.innerHTML = output;

      }, 1000);

    },

    createPost(post) {

      return new Promise((resolve, reject) => {

        setTimeout(() => {

          this.usersData.push(post);
          this.errorAPI = false;

          // If error reject promise
          if(!this.error) {
            resolve()
          } else {
            reject(this.errorText);
            this.errorAPI = true;
          }

        }, 2000);

      });


    }







    // https://jsonplaceholder.typicode.com/albums/1/photos
    // https://jsonplaceholder.typicode.com/albums/1/photos

  }
}

</script>
