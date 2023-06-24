<script setup>
import RegistrationFlow from '../components/RegistrationFlow.vue'
import FooterCustom from '../components/FooterCustom.vue'
</script>

<template>
  <div class="register">
    <RegistrationFlow />
    <FooterCustom />
  </div>
</template>

<style scoped>
.register {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
