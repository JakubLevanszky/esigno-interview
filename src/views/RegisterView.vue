<script setup>
import RegistrationFlow from '../components/RegistrationFlow.vue'
</script>

<template>
  <div class="register">
    <RegistrationFlow />
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .register {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
