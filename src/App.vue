<template>
  <v-app>
    <v-app-bar
      app
      color="green"
      dark
    >
      <div class="d-flex align-center">
        <span style="font-size: 30px;">
          golfways
        </span>
      </div>
    </v-app-bar>

    <v-content>
      <Search v-show="activeBtn == 0" />
      <List v-show="activeBtn == 1" />
      <Favorite v-show="activeBtn == 2" />
      <Setting v-show="activeBtn == 3" />
    </v-content>

    <v-footer
      padless
      fixed
        >
      <v-bottom-navigation
        :value="activeBtn"
        color="green"
      >
        <v-btn v-for="(link, index) in links" :key="index" @click="activeBtn = index">
          <span>{{ link.name }}</span>
          <v-icon>{{ link.icon }}</v-icon>
        </v-btn>
      </v-bottom-navigation>
    </v-footer>
  </v-app>
</template>

<script>
import Search from './components/pages/Search';
import List from './components/pages/List';
import Favorite from './components/pages/Favorite';
import Setting from './components/pages/Setting';

export default {
  name: 'App',

  components: {
    Search,
    List,
    Favorite,
    Setting,
  },

  data() {
    return {
      links: [
        { name: 'Search', icon: 'mdi-magnify' },
        { name: 'List', icon: 'mdi-format-list-bulleted' },
        { name: 'Favorite', icon: 'mdi-heart' },
        { name: 'Setting', icon: 'mdi-cog' },
      ],
      activeBtn: 0,
    }
  },
};
</script>
