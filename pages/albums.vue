<template>

  <div>
    <!-- https://jsonplaceholder.typicode.com/albums?userId=1 -->
    <!-- https://jsonplaceholder.typicode.com/photos?albumId=1 -->
    <!-- https://jsonplaceholder.typicode.com/photos?albumId=1?url[0] -->
  <v-container>
    <h1>Album page</h1>

    <div>
      <a href="/">Home</a>
    </div>
    <v-row>
      <v-col
        v-for="(single, index) in user" :key="index"
        xl="2"
        lg="3"
        md="4"
        sm="6"
      >
        <v-hover>
          <template #default="{ hover }">
              <v-card
                class="mx-auto"
                max-width="344"
              >
                <v-img
                  src="https://cdn.vuetifyjs.com/images/cards/sunshine.jpg"
                  height="200px"
                ></v-img>

                <v-card-subtitle>
                  {{single.title}}
                </v-card-subtitle>

                <v-fade-transition>
                  <v-overlay
                    v-if="hover"
                    absolute
                    color="#036358"
                  >
                    <v-btn
                      :to="'/photos?albumId=' + single.id + '/'"
                    >See all pictures</v-btn>
                  </v-overlay>
                </v-fade-transition>

              </v-card>
          </template>
        </v-hover>

      </v-col>
    </v-row>
  </v-container>

  </div>
</template>

<script>


export default {

  async asyncData({ $axios, error, query}) {

    try {

    // Call for all albums for user ID

    const { data } = await $axios.get('https://jsonplaceholder.typicode.com/albums?userId=' + query.userId)

    return {
      user: data
    }

    } catch(e) {

        // If API is down
        error({
          statusCode: 503,
          message: 'Unable to fetch API for user ID #' + query.userId
        })
      }

  },

  data() {
    return {
      par: 0
    }
  },

  head() {
    return {
      title: 'Album for ' + this.user.name,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Album #' + this.user.userId
        }
      ]
    }
  }
}
</script>
