<template>
<div>
<ul>
  <li v-for="(user, i) in usersData" :key="i">
    {{user.name}}

  </li>
</ul>

<h1>List of User ID</h1>
{{listOfUserIds}}

<h1>Random Album by ID - Num: {{albumsNumbers}}</h1>
{{albumsData}}

<h1>Random User - Num: {{usersNumber}}</h1>
{{usersData}}


<h1>Promises testing</h1>
{{newArr}}
</div>

</template>

<script>

import _ from 'lodash';

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

      arr: ['test', 'test2', 'test3'],
      newArr: [],
      data2: [],
      usersData: [],
      albumsData: [],
      usersNumber: [],
      randomAlbum: [],
      albumsNumbers: [],
      listOfUserIds:  []
    }
  }, computed: {

  },

  // Async getting all the API calls

  created() {

    this.generalCounter(this.usersData)
    this.albumCounters(this.albumsData)

    // this.getAlbumDataById()


  }, updated() {

  },

  mounted() {

  },

  methods: {

      // Users Array counter
      arrayCounterUsers(arr2) {

        return new Promise((resolve) => {
          const usersNummberAdd = arr2.length;
          resolve (usersNummberAdd);
        });

      },

      // Random user
      arrayRandomNumber(arr3) {
        return new Promise((resolve) => {
          const userRan = _.random(1,arr3.length)
          resolve (userRan)
        })
      },


      // Testing find
      arrayRandomAlbumNumberById(arr4) {

       return new Promise((resolve) => {

          const found = arr4.find(e => e.userId)

          // const userRan = _.random(1,arr4.length)
          resolve (found)
        })
      },

      // List of user id
      gettingListOfIds(arr5) {
       return new Promise((resolve) => {

          const found = arr5.map((obj) => obj.id)

          // const userRan = _.random(1,arr4.length)
          resolve (found)
        })
      },

      // List of obejct UserID and list of albums

      // Random album number by ID
      // getRandomAlbumNumberByUserId(id) {

      //   return new Promise((resolve) => {

      //     const albumDataById = this.$axios.get('https://jsonplaceholder.typicode.com/albums/' + id);
      //     const albumData = albumDataById.data

      //     resolve (albumData)
      //   })

      // },

      async albumCounters(x) {
          const albumNumber = await this.arrayCounterUsers(x);
          const album2 = await this.arrayRandomAlbumNumberById(x);

          this.albumsNumbers.push({'allAlbums': albumNumber}, {'id': album2})
      },

      async generalCounter(x) {

          const usersNumber2 = await this.arrayCounterUsers(x);
          const randomUser = await this.arrayRandomNumber(x);
          const album3 = await this.gettingListOfIds(x);
          this.usersNumber.push({'allUsers': usersNumber2}, {randomUser});
          this.listOfUserIds.push(album3);

        // });

        // for (const elem of x) {
        //   const newElem = await this.concatenator(elem);
        //   const newE2 = await this.arrayCounterUsers(elem);
        //   this.newArr.push({newElem, newE2})
        // }
      },

      async getAlbumDataById(id) {
        const randomAlbum = await this.getRandomAlbumNumberByUserId(id);
        this.randomAlbum.push(randomAlbum)
      }

      // async arrayCounter(arr) {


      //     const usersNummber = warr.length;

      //     return usersNummber;

      // }




    // https://jsonplaceholder.typicode.com/albums/1/photos
    // https://jsonplaceholder.typicode.com/albums/1/photos

  }
}

</script>
