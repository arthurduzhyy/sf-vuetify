<template>
  <v-app>
    <v-app-bar app fixed>
      <v-app-bar-nav-icon @click="drawer = !drawer">
        <v-icon>mdi-menu</v-icon>
      </v-app-bar-nav-icon>
      <v-app-bar-title>My Project</v-app-bar-title>
    </v-app-bar>

    <v-navigation-drawer v-model="drawer" app>
      <v-list>
        <v-list-item
            v-for="item in items"
            :key="item.title"
            link
            :to="item.to"
            :title="item.title">
          <template v-slot:prepend>
            <v-icon>{{ item.icon }}</v-icon>
          </template>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script setup>
import { ref } from 'vue'

const drawer = ref(null)

const items = ref([
  { title: 'List', to: '/', icon: 'mdi-format-list-checks' },
  { title: 'About', to: '/about', icon: 'mdi-help-box' }
])

const right = ref(false)
</script>

<style>

</style>