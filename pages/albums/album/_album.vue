<template>

<!--This is Albums by User-->

<div>
   The id is {{$route.params.album}}

   {{photosByIdData}}

</div>

</template>

<script>

export default {

      // Nuxt method for data fetching - asyncData

  async asyncData({ $axios, params }) {

    // https://jsonplaceholder.typicode.com/albums/1/photos
    
    const res = await $axios.get(`https://jsonplaceholder.typicode.com/albums/${params.album}/photos`)
    const photosByIdData = res.data
  
    return { photosByIdData }
  },

  data() {
    return {
      albumsByIdData: [],
      alertFired: null,
      photosByIdData: []
    }
  }

}
</script>
