<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :clipped="clipped"
      fixed
      app
    >

    <v-img src="app-logo-white.png" class="mx-auto my-8" contain max-width="150"></v-img>

      <v-list shaped>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router color="primary"
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <template v-slot:append>
        <div class="pa-2 mx-auto">
          <v-card-text>ExaQuo - Beta Release 2.3</v-card-text>
        </div>
      </template>

    </v-navigation-drawer>

    <!-- v-app-bar flat
      :clipped-left="clipped"
      fixed color="transparent"
      app class="my-2"
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-toolbar-title>{{ title }}</v-toolbar-title>
      <v-spacer />
    </v-app-bar -->

    <v-main>
      <v-container>
        <Nuxt @AppColor="ThemeChanger"/>
      </v-container>
    </v-main>

    <v-bottom-navigation
      fixed grow v-model="bottomNav"
    >
      <v-btn :color="themeColor" class="py-6" height="100%" to="/">
        <v-icon>mdi-view-dashboard</v-icon>
      </v-btn>

      <v-btn class="py-6" height="100%" to="search">
        <v-icon :color="themeColor">mdi-magnify</v-icon>
      </v-btn>

      <v-btn class="py-6" height="100%" @click="DevSnackbar = true">
        <v-icon :color="themeColor">mdi-bell-outline</v-icon>
      </v-btn>

      <v-btn @click.stop="drawer = !drawer" class="py-6" height="100%">
        <v-icon :color="themeColor">mdi-square-rounded-badge-outline</v-icon>
      </v-btn>
    </v-bottom-navigation>

    <v-snackbar tile flat
      v-model="DevSnackbar"
    >
    <v-list-item class="ma-0 pa-0">
      Notification feature is currently under development
        <v-btn icon
          color="white"
          variant="text" class="ma-0 ml-2"
          @click="DevSnackbar = false"
        >
          <v-icon>mdi-close</v-icon>
        </v-btn>
      </v-list-item>
    </v-snackbar>

  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      DevSnackbar: false,
      themeColor: '#f9784b',
      bottomNav: 0,
      items: [
        {
          icon: 'mdi-book-open-variant',
          title: 'ExaQuo',
          to: '/'
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'About',
          to: '/about'
        },
        {
          icon: 'mdi-dev-to',
          title: 'Developers',
          to: '/developers'
        },
      ],
      right: true,
      title: 'ExaQuo'
    }
  },

  methods: {
      ThemeChanger(e) {
        this.themeColor = e
      },
  },
}
</script>

<style scoped>

</style>
