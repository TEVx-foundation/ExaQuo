<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :clipped="clipped"
      fixed
      app
    >
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

      <v-btn class="py-6" height="100%">
        <v-icon :color="themeColor">mdi-bell-outline</v-icon>
      </v-btn>

      <v-btn @click.stop="drawer = !drawer" class="py-6" height="100%">
        <v-icon :color="themeColor">mdi-square-rounded-badge-outline</v-icon>
      </v-btn>
    </v-bottom-navigation>

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
          to: '/custom'
        }
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
