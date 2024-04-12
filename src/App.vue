<script setup>
import { RouterLink, RouterView } from 'vue-router'
import Navigation from './components/Navigation.vue'
</script>

<template>
  <header>
    <Navigation />
  </header>

  <RouterView />
</template>
