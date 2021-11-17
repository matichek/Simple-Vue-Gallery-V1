<template>

<!--This is Albums by User-->

<div>
   The id is {{$route.params.albums}}

  <ul>
    <li v-for="(article, i) in articles" :key="i">

      {{article.title}}

    </li>
  </ul>
   

</div>

</template>

<script>

export default {

  data() {
    return {
      articles: [],
      articles1: null
    }
  },

  mounted() {

 
  },

  methods: {
      testF() {
          setTimeout(() => {

            this.articles1 = "Test ok"

          }, 1000)
    }
  }

}

</script>
