<template>

<!--This is Albums by User-->

<div>
   The id is {{$route.params.albums}}

   <h1>Album info</h1>
    {{MergedUsersAlbumUrlsArray}}




   <h1>All the photos</h1>
   {{photosResData}}


<h1>Albums by ID</h1>
{{albumsByIdData}}
</div>

</template>

<script>
import _ from 'lodash'

export default {

    // Nuxt method for data fetching - asyncData

  async asyncData({ $axios, params }) {
    
    const res = await $axios.get(`https://jsonplaceholder.typicode.com/users/${params.albums}/albums`)
    const albumsByIdData = res.data
  

    const photosRes = await $axios.get('https://jsonplaceholder.typicode.com/photos')
    const photosResData = photosRes.data

    return { photosResData, albumsByIdData }
  },

  data() {
    return {
      MergedUsersAlbumUrlsArray: [],
      albumsByIdData: [],
      randomPicture: []
    }
  },

  async created() {

    await this.PrepPhotosArray()
    await this.PrepNewArrayForAlbums()

  },

  methods: {
    PrepPhotosArray() {
      return new Promise((resolve, reject) => {
        this.photosResData = this.photosResData.map(function (item) {
          return {
            albumId: item.albumId,
            id: item.id,
            url: item.url
          }
        })

        if (!this.error) {
          resolve()
        } else {
          reject(this.errorText)
          this.errorAPI = true
        }
      })
    },

    PrepNewArrayForAlbums() {
      return new Promise((resolve, reject) => {
        this.MergedUsersAlbumUrlsArray = this.albumsByIdData.map((obj) => {
          return {
            userId: obj.userId,
            id: obj.id,
            title: obj.title,
            p_id: this.getListOfPicturesBy(obj.id),
            random_id: this.getRandomValueFromArray(this.getListOfPicturesBy(obj.id)),
            random_url: this.getUrlBasedOnPictureId(obj.id)

          }
        })

        if (!this.error) {
          resolve()
        } else {
          reject(this.errorText)
          this.errorAPI = true
        }
      })
    },
    getListOfPicturesBy(getId) {
      // Getting specific object filtered by Id
      const albumListArray = this.photosResData.filter((e) => e.albumId === getId)

      // Retaining only the Ids of the albums
      const albumListArray2 = albumListArray.map(function (item) {
        return item.id
      })

      return albumListArray2
    },

      // Get random value from array
    getRandomValueFromArray(arr) {
      // Random value between first item in the array and last item
      const randomA = _.random(arr[0], arr[arr.length - 1])

      // Assined another variable to picking random urls in line 183
      this.randomPicture = randomA

      return randomA
    },

    // Get url based on id of pa picture
    getUrlBasedOnPictureId(getId) {

      const picListArray = this.photosResData.filter((e) => e.id === getId)

      // Retaining only the Ids of the albums
      const picListArray2 = picListArray.map(function (item) {
        return item.url
      })

      return picListArray2

    }

  }

}
</script>
