<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-btn
        icon
        @click.stop="miniVariant = !miniVariant"
      >
        <v-icon>mdi-{{ `chevron-${miniVariant ? 'right' : 'left'}` }}</v-icon>
      </v-btn>
      <v-btn
        icon
        @click.stop="clipped = !clipped"
      >
        <v-icon>mdi-application</v-icon>
      </v-btn>

      <v-btn
        icon
        @click.stop="fixed = !fixed"
      >

        <v-icon>mdi-minus</v-icon>
      </v-btn>
      <v-toolbar-title v-text="title" />
      <v-spacer />

      <v-btn
      class="mx-2"
      fab
      dark
      small
      color="primary"
      @click="toggleTheme"
    >
      <v-icon dark>
        mdi-theme-light-dark
      </v-icon>
    </v-btn>

    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>

    <v-footer
      :absolute="!fixed"
      app
    >
      <span>Matija &copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      switchText: 'Switch to dark',
      items: [
        {
          icon: 'mdi-home',
          title: 'Home',
          to: '/'
        },
        {
          icon: 'mdi-table-eye',
          title: 'Dashboard',
          to: '/dashboard'
        },
        {
          icon: 'mdi-apps',
          title: 'Sitemap',
          to: '/sitemap'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Vue Gallery with Vuetify'

    }
  },
  methods: {
        toggleTheme() {
          this.$vuetify.theme.dark = !this.$vuetify.theme.dark
        }
      }
}
</script>
