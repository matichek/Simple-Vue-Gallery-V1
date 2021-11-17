<template>

<!--This is Albums by User-->

<div>
   The id is {{$route.params.albums}}

   {{albumsByIdData}}
</div>

</template>

<script>

export default {

  // Nuxt method for data fetching - asyncData

  async asyncData({$axios, params}) {

    // Get users object
    const res = await $axios.get(`https://jsonplaceholder.typicode.com/users/${params.albums}/albums`)
    const albumsByIdData = res.data

    return {albumsByIdData}
  },


  data() {
    return {
      albumsByIdData: []
    }
  }

}
</script>
