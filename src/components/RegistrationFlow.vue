<template>
  <section v-if="step === 1" class="baseform">
    <h1>Registrace</h1>
    <h3>Máte účet? <a href="#">Přihlásit se</a></h3>
    <span
      >Na e-mail Vám bude zaslán ověřovací kód. Po ověření prosím pokračujte v dalších krocích
      registrace.</span
    >
    <form @submit.prevent="regEmailSubmit">
      <InputCustom v-model="email" type="email" label="E-mail" required />
      <button type="submit">Pokračovat</button>
    </form>
  </section>

  <section v-if="step === 2" class="baseform">
    <h1>Zadejte kód</h1>
    <a href="#">Zpět na přihlášení</a>
    <span>
      Potřebujeme ověřit vaši identitu, zadejte prosím ověřovací PIN kód, který jsme vám poslali na
      e-mail {{ email }}.
    </span>
    <form @submit.prevent="pincodeSubmit">
      <PinCode :digit-count="4" @update:otp="pincodeValue = $event" />
      <button type="submit">Ověřit a pokračovat</button>
    </form>
    <hr />
    <span>Nepřišel Vám e-mail? Prosím zkontrolujte spam.</span>
    <a href="#">Zaslat znovu kód</a>
  </section>

  <section v-if="step === 3" class="baseform">
    <h1>Tvorba hesla</h1>
    <a href="#">Zpět na přihlášení</a>
    <span
      >Děkujeme za vaše ověření. Nyní si můžete nastavit heslo. Dbejte prosím, aby heslo bylo
      dostatečně silné a neobsahovalo vaše iniciály či jiné snadno uhodnutelné slova.
    </span>
    <form @submit.prevent="">
      {{ passwd }}
      <InputCustom v-model="passwd" type="password" label="Heslo" required />
      <button type="submit">Vytvořit a pokračovat</button>
    </form>
    <p>Obsahuje alespoň 8 znaků</p>
    <p>Obsahuje jak malá písmena (a-z), tak velká (A-Z)</p>
    <p>Obsahuje alespoň jednu číslici (0-9)</p>
    <p>Obsahuje alespoň jeden speciální znak (@, #, /)</p>
    <p>Neobsahuje Vaši e-mailovou adresu</p>
    <p>Neobsahuje Vaše jméno a příjmení</p>
    <!-- Jmeno a prijmeni je az v dalsim kroku - nelze overit-->
  </section>
</template>

<script setup>
import InputCustom from './InputCustom.vue'
import PinCode from './OTP.vue'
import { ref } from 'vue'
import axios from 'axios'

const step = ref(1)
const email = ref('')
const pincodeValue = ref('')
const passwd = ref('')
const name = ''
const surname = ''
const company = ''

function regEmailSubmit() {
  axios
    .post(
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
      respone.status === 200 ? (step.value = 2) : (step.value = 1)
    })
}

function pincodeSubmit() {
  console.log(pincodeValue.value)
  axios
    .post(
      `/api/registration/${email.value}/verify`,
      {
        code: pincodeValue.value
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
      respone.status === 200 ? (step.value = 3) : console.error('Bad pincode')
    })
}
</script>

<style scoped></style>
