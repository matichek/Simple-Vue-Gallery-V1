<template>
<v-container>
  <h1>Index Home</h1>

  <div>All users: </div>
  <v-card v-for="(user, index) in users" :key="index">
    ID: {{user.id}}, {{user.name}}
  </v-card>

</v-container>
</template>

<script>

/* Example of SEO info */

export default {

async asyncData({ $axios, error }) {

    try {
      const response = await $axios.get('https://jsonplaceholder.typicode.com/users')

      return {
        users: response.data
      }

    } catch(e) {

        // If API is down
        error({
          statusCode: 503,
          message: 'Unable to fetch API. Try a bit later. Thanks. ;)'
        })
      }

  },

  data() {
    return {
      usersNumber: 0
    }
  },
  head() {
    return {
      title: 'Matijas Gallery Test with Nuxt.js',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Where you can find gallery desc'
        }
      ]
    }
  }
}

</script>
