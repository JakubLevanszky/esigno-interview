<script setup>
import InputCustom from './InputCustom.vue'
import { ref } from 'vue'
import axios from 'axios'

const methods = {
  handleSubmit() {
    console.log('handlesubmit')
    axios
      .post(
        // 'https://cors-anywhere.herokuapp.com/https://userhw.sandbox.esigno.io/registration',
        '/api/registration',
        {
          email: email.value
        },
        {
          headers: {
            Accept: '*/*',
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*'
          }
        }
      )
      .then((respone) => {
        const data = respone.data
        console.log(respone)
        console.log(data)
      })
  }
}

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
    <form @submit.prevent="methods.handleSubmit">
      <InputCustom v-model="email" type="email" label="E-mail" required />
      <button type="submit">Pokračovat</button>
    </form>
  </div>
</template>

<style scoped></style>
