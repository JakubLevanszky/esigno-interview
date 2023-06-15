<script setup>
import InputCustom from './InputCustom.vue'
import { ref } from 'vue'

const email = ref('')
</script>

<template>
  <div class="greetings">
    <h1>Registrace</h1>
    <h3>Máte účet? <a href="#">Přihlásit se</a></h3>
    <span
      >Na e-mail Vám bude zaslán ověřovací kód. Po ověření prosím pokračujte v dalších krocích
      registrace.</span
    >
    <form>
      <InputCustom v-model="email" type="email" label="E-mail" required />
      <button type="submit">Pokračovat</button>
    </form>
  </div>
</template>

<style scoped></style>
