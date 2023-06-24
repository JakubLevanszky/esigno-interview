<template>
  <section v-if="step === 1" class="login-wrapper">
    <div class="login-header">
      <h1>Přihlášení</h1>
      <span>Jste nový uživatel? <RouterLink to="/register">Registrovat se</RouterLink></span>
    </div>

    <form @submit.prevent="loginSubmit">
      <InputCustom v-model="email" type="email" label="E-mail" required />
      <InputCustom v-model="passwd" type="password" label="Heslo" required />
      <button type="submit">Přihlásit se</button>
    </form>
  </section>

  <section v-if="step === 2" class="login-wrapper center">
    <img src="./icons/robot_happy.png" alt="image of happy robot" class="robot-image" />
    <h1>Přihlášení proběhlo úspěšně</h1>
    <span>Prosím vyčkejte na přesměrování....</span>
  </section>

  <section v-if="step === 3" class="login-wrapper center">
    <img src="./icons/robot_sad.png" alt="image of sad robot" class="robot-image" />
    <h1>Přihlášení se nezdařilo</h1>
    <span
      >Přihlášení neproběhlo z důvodu chyby na serveru. Prosím opakujte proces znovu nebo nás
      kontaktujte na e-mail <strong>podpora@esigno.com</strong>.</span
    >
    <button>Zkusím to znovu</button>
  </section>
  <RouterView />
</template>

<script setup>
import { RouterLink, RouterView } from 'vue-router'
import InputCustom from './InputCustom.vue'
import { ref } from 'vue'
import axios from 'axios'

const step = ref(1)
const email = ref('')
const passwd = ref('')

function loginSubmit() {
  axios
    .post(
      '/api/users',
      {
        email: email.value,
        password: passwd.value
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
      respone.status === 200 ? (step.value = 2) : console.error('Error occured')
    })
    .catch((error) => {
      error.response.status === 401 ? (step.value = 3) : console.log(error)
    })
}
</script>

<style scoped>
.login-wrapper {
  display: flex;
  flex-direction: column;
  gap: 32px;
  width: 463px;
  padding: 32px;
  background-color: var(--color-background-form);
  border-radius: 8px;
}

.center.login-wrapper {
  align-items: center;
}

.login-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  align-self: stretch;
}

.robot-image {
  height: 250px;
  width: 190px;
}
</style>
