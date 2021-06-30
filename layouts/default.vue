<template>
  <v-app dark>
    <v-navigation-drawer
      class="deep-purple accent-4"
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
      <v-list rounded>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
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
      <v-app-bar-nav-icon @click="drawer = !drawer" />
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

    </v-app-bar>
    <v-main>
      <v-container>

        <Nuxt v-if="dataLoaded" :nuxt-child-key="posts"/>

      </v-container>

    </v-main>

    <v-footer
      :absolute="true"
      app
    >
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
import axios from "axios";
export default {
  created() {
    this.getPosts()
  },
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/'
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Inspire',
          to: '/inspire'
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Filters',
          to: '/filters'
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Dashboard',
          to: '/dashboard'
        }

      ],
      posts:[],
      dataLoaded:false,
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Vuetify.js'
    }
  },
  methods: {
    async getPosts() {
      let response = await axios.get("http://challenge.iperasolutions.com/filters");
      response.data.forEach( item => {
        if(item.type === 'select'){
          item.value = null
        } else if( item.type === 'text') {
          item.value = ''
        } else {
          item.value = new Date().toISOString().slice(0,10)
        }
        this.posts.push(item)
      })
      this.dataLoaded = true;
    },

  },
}
</script>
