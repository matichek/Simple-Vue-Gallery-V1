<template>
<div>
<v-container>

  <h1>Test2</h1>
    <ul>
    <li v-for="(user, index) in users" :key="index">
      {{user.name}} <br>

      <!-- BIg problem doesn't load properly -->
      {{ getAlbums(user.id) }}

      {{ getTest(user.id) }}

      </li>
  </ul>

  <h2>Test 2</h2>
<!--{{album1}}-->
{{getTest(1)}}


<h3>Test async22222222222222222222</h3>


<div v-for="(i,index) in arrayOne" :key="index">

<li>i.name</li>

</div>

{{array1}}


</v-container>

</div>
</template>

<script>


export default {

  data() {
    return {
      users: [],
      albumUrl: [],
      url: "",
      users1: [],
      album1: [],
      albumfull: [],
      data2: null,
      arrayOne: [],
      arrayTwo: [],
      array1: []
    }
  },

  async created() {

    await this.getUsers()
   /*  await this.getUsersList()
    await this.getAlbumsList() */

  }, beforeMount() {

  },

/*   mounted: {
    getAlbums(UserId) {
      this.$axios
        .get('https://jsonplaceholder.typicode.com/albums/'+ UserId + '/photos')
        .then((res2) => {
          this.albumUrl = res2.data[UserId].url
        })

    }
  }, */

  methods: {

    getMainCall: ()=> {

      const vm = this

      vm.$axios.get('https://jsonplaceholder.typicode.com/users').then(response => {
      vm.arrayOne = response.data;

      const array1 = vm.arrayOne;

      return array1




    /*   this.arrayOne.forEach(function(id) {
        this.arrayTwo.push(id);
      }) */



 /*      this.arrayOne.forEach(function(id) {
        copyItems.push(id)
      })

      this.arrayOne = response.data
      axios.get('/user/12345/' + this.arrayOne.name + '/permissions').then(
       response => this.arrayTwo = response.data
     ); */


  });

    },



    getUsers() {

        return this.$axios
          .get('https://jsonplaceholder.typicode.com/users')
          .then((res) => {
            this.users = res.data

            /* this.getAlbums(this.users.id) */
            /* this.getAlbums() */
          })

    },
    getAlbums(UserId) {

      const data2 = this.$axios.get(`https://jsonplaceholder.typicode.com/albums/`+UserId).data;

      return "This is a test222" + data2;

    },

    getTest(id) {

      return "This is a test: " + id

    },

    getUsersList() {
      this.$axios
        .get('https://jsonplaceholder.typicode.com/users')
        .then((res2) => {
          this.users1 = res2.data
        })
    },
    getAlbumsList() {
      this.$axios
        .get('https://jsonplaceholder.typicode.com/albums/')
        .then((res2) => {
          this.album1 = res2.data
        })

    }

  }



}


</script>

