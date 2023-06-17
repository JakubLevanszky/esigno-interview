<script setup>
import { RouterLink, RouterView } from 'vue-router'
import Aside from './components/Aside.vue'
</script>

<template>
  <div class="wrapper">
    <Aside />
    <main>
      <RouterLink to="/">Login</RouterLink>
      <RouterLink to="/register">Register</RouterLink>
    </main>
  </div>
  <RouterView />
</template>

<style scoped></style>
