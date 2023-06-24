<script setup>
import { RouterView } from 'vue-router'
import Aside from './components/Aside.vue'
</script>

<template>
  <div class="wrapper">
    <Aside />
  </div>
  <RouterView />
</template>

<style scoped></style>
