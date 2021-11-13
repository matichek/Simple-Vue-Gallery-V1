<template>
<div>
<v-container>

<h1>Random number</h1>
{{randomNumber}}

<h1>Is new var loading?</h1>
{{mergedObject}}

<h1>Modified Album object - preparing for merge</h1>

{{albumsArrayPrepared}}

<h1>Users Prepared</h1>
{{usersArrayPrepared}}

<h1>Appding elements stuff</h1>

<ul id="listAlbumsTest">
  <ul id="listOfAlbumsId">
    <li v-for="(i, index) in usersArray" :key="index">{{i.name}}

      <ul>

      </ul>

    </li>
  </ul>
</ul>



<h1>Users list</h1>
<!--
<ul id="listOfAlbumsId">
  <li v-for="(i, index) in usersArray" :key="index">{{i.name}}

    <ul>

        {{filteredAlbumsById(i.id)}}

        {{filteredAlbumsById(i.id)}}

        <li></li>

    </ul>

  </li>
</ul>
-->

<h1>Merged Arrays</h1>
{{arr1}} + {{arr2}} = <br>
{{arrMerge}}
<h1>Users Array:</h1>
{{usersArray}}

<h1>Albums Array:</h1>
{{albumsArray}}

</v-container>

</div>
</template>

<script>
import _ from 'lodash';

export default {

  data() {
    return {
      usersArray: [],
      albumsArray: [],
      albumsArrayById: [],
      albumsNames: [],
      albumsArrayPrepared: [],
      usersArrayPrepared: [],
      mergedObject: [],
      randomNumber: null,


      arr1: [
        {id: 1, name: 'Matija'},
        {id: 2, name: 'Matej'}
      ],
      arr2: [
        {userId: 1, username: 'matichek'},
        {userId: 2, username: 'utrip'}
      ],
      arrMerge : []
    }
  }, computed: {



    },


  mounted() {
    this.prepareAlbums()
  },

  async created() {

    await this.getUsers()
    await this.getAlbums()

   /*  await this.getUsersList()
    await this.getAlbumsList() */

  }, updated() {

  },

  methods: {

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



        // this.mergedObject = this.usersArrayPrepared.map(v => ({ ...v, ...this.albumsArrayPrepared.find(sp => sp.userId === v.userId) }));

        // this.mergedObject = _.merge(this.usersArrayPrepared, this.albumsArrayPrepared)

        this.mergedObject =  _(this.albumsArrayPrepared)
          .groupBy('userId')
          .map((values, key) => {
            if (values.length === 1) {
              values[0].album_id = [values[0].albumd_id];
              return values[0];
            }
            return _.mergeWith(...values, this.customizer);
          })
          .value();




      })
    },

    customizer(objValue, srcValue, key, fourth) {
      if (key === 'server') {
        if (!_.isArray(objValue)) objValue = [objValue];
        return objValue.concat(srcValue);
      }
    },
    prepareAlbums() {

      // const albumsArray3 = this.usersArrayPrepared
      // return albumsArray3

    }

    // getAlbums() {

    //     return this.$axios
    //       .get('https://jsonplaceholder.typicode.com/albums')
    //       .then((res) => {
    //         this.albumsArray = res.data
    //       })
    // },

    // getUsers() {

    //     return this.$axios
    //       .get('https://jsonplaceholder.typicode.com/users')
    //       .then((res) => {
    //         this.usersArray = res.data
    //       })

    // },
    // getAlbums() {

    //     return this.$axios
    //       .get('https://jsonplaceholder.typicode.com/albums')
    //       .then((res) => {
    //         this.albumsArray = res.data
    //       })
    // },

    // filteredAlbumsById(id) {

    //     const albumsArrayById = this.albumsArray.filter(function(item) { return item.userId === id});

    //     const albumsArrayTitles = albumsArrayById.map(a => a.title);

        // const albumsArrayTitlesHtml = albumsArrayTitles.split("|");

        // Lets display list of album for the user id
        // document.getElementById("listOfAlbumsId").innerHTML = "Test" + albumsArrayById
        // document.getElementById("listOfAlbumsId").innerHTML = "<li><ul>"+albumsArrayById+"</ul></li>"

        // return {albumsArrayTitles}

   // },


        // return this.albumsArray.filter(c => c.id.includes(userId) > -1)
       // const albumsArrayById = this.albumsArray.filter(function(item) { return item.userId === id})

        // return albumsArrayById


    // filteredAlbumsById(id) {
    //     // return this.albumsArray.filter(c => c.id.includes(userId) > -1)
    //     const albumsArrayById = this.albumsArray.filter(function(item) { return item.userId === id})

    //     return ""
    // },

    // mergeTest() {
    //   const map = new Map();
    //   this.arr1.forEach(item => map.set(item.id, item));
    //   this.arr2.forEach(item => map.set(item.userId, {...map.get(item.nameId), ...item}));

    //   const arrMerge = Array.from(map.values());

    //   return arrMerge

    // }

  }



}


</script>

