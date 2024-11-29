<template>
  <v-app>
    <v-navigation-drawer
      v-model="drawer"
      app
      dark
      :mini-variant.sync="miniVariant"
      permanent
      color="primary darken-1"
    >
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title class="text-h6"> My App </v-list-item-title>
          <v-list-item-subtitle> Navigation </v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>

      <v-divider></v-divider>

      <v-list dense nav>
        <v-list-item v-for="item in items" :key="item.title" :to="item.to" link>
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <template v-slot:append>
        <v-list-item @click.stop="miniVariant = !miniVariant">
          <v-list-item-icon>
            <v-icon
              >{{ miniVariant ? 'mdi-chevron-right' : 'mdi-chevron-left'
              }}</v-icon
            >
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title>Collapse</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </template>
    </v-navigation-drawer>

    <v-app-bar app color="primary" dark elevate-on-scroll>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>My App</v-toolbar-title>
    </v-app-bar>

    <v-main>
      <v-container fluid>
        <router-view></router-view>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
  export default {
    name: 'AppNavigation',
    data: () => ({
      drawer: true,
      miniVariant: false,
      items: [
        { title: 'Dashboard', icon: 'mdi-view-dashboard', to: '/' },
        {
          title: 'Trade Execution',
          icon: 'mdi-chart-line',
          to: '/trade-execution',
        },
      ],
    }),
  }
</script>

