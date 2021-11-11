/* https://jsonplaceholder.typicode.com/users/ */

<template>
<v-container>

  <h1 class="mb-5">Users: ({{users.length}})</h1>

  <v-row>
    <v-col
      v-for="(user, index) in users" :key="index"
      xl="2"
      lg="3"
      md="4"
      sm="6"
    >
      <v-card
        class="mx-auto"
        max-width="344"
      >
        <v-img
          src="https://cdn.vuetifyjs.com/images/cards/sunshine.jpg"
          height="200px"
        ></v-img>

        <v-card-title>
          {{user.name}}
        </v-card-title>

        <v-card-subtitle>
          {{user.username}}
        </v-card-subtitle>

        <v-card-actions>
          <v-btn
            :to="'/albums?userId=' + user.id"
            color="orange lighten-2"
            text
          >
            View Album
          </v-btn>

          <v-spacer></v-spacer>
        </v-card-actions>

      </v-card>

    </v-col>
  </v-row>

  <div v-if="users.length">
    <span>{{users.length}}</span>
  </div>
  <div>
  {{users}}
  </div>

</v-container>
</template>

<script>

/* Example of SEO info */

export default {

  async asyncData({ $axios, error }) {

      try {

      const { data } = await $axios.get('https://jsonplaceholder.typicode.com/users')

      return {
        users: data
      }

    } catch(e) {

        // If API is down
        error({
          statusCode: 503,
          message: 'Unable to fetch API. Try a bit later. Thank you ;)'
        })
      }

  },

  data() {
    return {
      usersNumber: 0,
      apiUrl: "https://jsonplaceholder.typicode.com"
    }
  },
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
  }
}

</script>
