<script setup>
import LoginFlow from '../components/LoginFlow.vue'
import FooterCustom from '../components/FooterCustom.vue'
</script>

<template>
  <main>
    <div class="login">
      <LoginFlow />
      <FooterCustom />
    </div>
  </main>
</template>

<style scoped>
.login {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
</style>
