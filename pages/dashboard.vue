

<template>

<div>
  <v-container>
    <v-row>
    <v-col>
      <v-data-table
          :headers="headers"
          :items="users"
          :items-per-page="10"
          class="elevation-1"
        ></v-data-table>

        </v-col>

      </v-row>

    </v-container>

</div>
</template>

<script>



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
        headers: [
          {
          text: 'Id',
          align: 'start',
          sortable: false,
          value: 'id'
          },
          {
            text: 'Name', value: 'name'
          },
          {
            text: 'Username', value: 'username'
          },
          {
            text: 'Email', value: 'email'
          }
        ]
      }

    }
}
</script>
