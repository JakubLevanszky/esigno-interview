<template>
  <section v-if="step === 1" class="regform-wrapper">
    <div class="regform-header">
      <h1>Registrace</h1>
      <span>Máte účet? <RouterLink to="/">Přihlásit se</RouterLink></span>
    </div>
    <span
      >Na e-mail Vám bude zaslán ověřovací kód. Po ověření prosím pokračujte v dalších krocích
      registrace.</span
    >
    <form @submit.prevent="regEmailSubmit">
      <InputCustom v-model="email" type="email" label="E-mail" required />
      <button type="submit">Pokračovat</button>
    </form>
  </section>

  <section v-if="step === 2" class="regform-wrapper">
    <div class="regform-header">
      <h1>Zadejte kód</h1>
      <RouterLink to="/">Zpět na přihlášení</RouterLink>
    </div>
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

  <section v-if="step === 3" class="regform-wrapper">
    <div class="regform-header">
      <h1>Tvorba hesla</h1>
      <RouterLink to="/">Zpět na přihlášení</RouterLink>
    </div>
    <span
      >Děkujeme za vaše ověření. Nyní si můžete nastavit heslo. Dbejte prosím, aby heslo bylo
      dostatečně silné a neobsahovalo vaše iniciály či jiné snadno uhodnutelné slova.
    </span>
    <form @submit.prevent="passwordCreatedSubmit">
      <InputCustom
        v-model="passwd"
        type="password"
        label="Heslo"
        v-on:keyup="isPassword($event)"
        required
      />
      <button type="submit" :disabled="!isDisabled">Vytvořit a pokračovat</button>
    </form>
    <p :class="{ valid: is8CharLongRef, invalid: !is8CharLongRef }">Obsahuje alespoň 8 znaků</p>
    <p :class="{ valid: isLowerAndUpperCase, invalid: !isLowerAndUpperCase }">
      Obsahuje jak malá písmena (a-z), tak velká (A-Z)
    </p>
    <p :class="{ valid: isNumberRef, invalid: !isNumberRef }">
      Obsahuje alespoň jednu číslici (0-9)
    </p>
    <p :class="{ valid: isSpecialRef, invalid: !isSpecialRef }">
      Obsahuje alespoň jeden speciální znak (@, #, /)
    </p>
    <p :class="{ valid: !isNotYourEmail, invalid: isNotYourEmail }">
      Neobsahuje Vaši e-mailovou adresu
    </p>
    <p class="valid">Neobsahuje Vaše jméno a příjmení</p>
    <!-- Jmeno a prijmeni je az v dalsim kroku - nelze overit-->
  </section>

  <section v-if="step === 4" class="regform-wrapper">
    <h1>Údaje k registrovanému účtu</h1>
    <span>Pro úspěšnou registraci prosím vyplňte následující údaje k vašemu účtu.</span>
    <form @submit.prevent="detailsSubmit">
      <InputCustom v-model="name" type="text" label="Jméno" required />
      <InputCustom v-model="surname" type="text" label="Příjmení" required />
      <InputCustom v-model="company" type="text" label="Společnost" required />
      <span
        >Registrací vyjadřujete souhlas s všeobecnými obchodními podmínkami a zásadami ochrany
        osobních údajů.</span
      >
      <button type="submit">Dokončit registraci</button>
    </form>
  </section>

  <section v-if="step === 5" class="regform-wrapper">
    <img src="./icons/robot_happy.png" alt="image of happy robot" />
    <h1>Registrace proběhla úspěšně</h1>
    <span>Děkujeme váš profil byl úspěšně založen. Nyní můžete přejít do portálu Esigno.</span>
    <button>Přejít do Esigno</button>
  </section>

  <section v-if="step === 6" class="regform-wrapper">
    <img src="./icons/robot_sad.png" alt="image of sad robot" />
    <h1>Registrace se nezdařila</h1>
    <span
      >Vás profil se nepodařilo založit z důvodu chyby na serveru. Prosím opakujte proces znovu nebo
      nás kontaktujte na e-mail <strong>podpora@esigno.com</strong>.</span
    >
    <button>Zkusím to znovu</button>
  </section>
  <RouterView />
</template>

<script setup>
import { RouterLink, RouterView } from 'vue-router'
import InputCustom from './InputCustom.vue'
import PinCode from './OTP.vue'
import { ref, computed } from 'vue'
import axios from 'axios'

const step = ref(1)
const email = ref('')
const pincodeValue = ref('')
const passwd = ref('')
const name = ref('')
const surname = ref('')
const company = ref('')
const isLowerAndUpperCase = ref(false)
const isNumberRef = ref(false)
const isSpecialRef = ref(false)
const is8CharLongRef = ref(false)
const isNotYourEmail = ref(true)
const isDisabled = computed(
  () =>
    isLowerAndUpperCase.value &&
    isNumberRef.value &&
    isSpecialRef.value &&
    is8CharLongRef.value &&
    !isNotYourEmail.value
)

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

function isPassword() {
  const isUppercase = /[A-Z]/.test(passwd.value)
  const isLowercase = /[a-z]/.test(passwd.value)
  const isNumber = /[0-9]/.test(passwd.value)
  const isSpecial = /[#?!@$%^&*-]/.test(passwd.value)
  const is8CharLong = passwd.value.length >= 8

  isUppercase && isLowercase
    ? (isLowerAndUpperCase.value = true)
    : (isLowerAndUpperCase.value = false)
  isNumber ? (isNumberRef.value = true) : (isNumberRef.value = false)
  isSpecial ? (isSpecialRef.value = true) : (isSpecialRef.value = false)
  is8CharLong ? (is8CharLongRef.value = true) : (is8CharLongRef.value = false)
  email.value === passwd.value ? (isNotYourEmail.value = true) : (isNotYourEmail.value = false)
}

function passwordCreatedSubmit() {
  axios
    .post(
      `/api/registration/${email.value}/password`,
      {
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
      respone.status === 200 ? (step.value = 4) : console.error('Password is incorrect')
    })
}

function detailsSubmit() {
  axios
    .post(
      `/api/registration/${email.value}/details`,
      {
        givenName: name.value,
        surname: surname.value,
        company: company.value
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
      respone.status === 200 ? (step.value = 5) : console.error('Details are incorrect')
      respone.status === 404 ? (step.value = 6) : console.error('Registarce se nezdarila')
    })
}
</script>

<style scoped>
h1 {
  font-size: 24px;
}

.regform-wrapper {
  display: flex;
  flex-direction: column;
  gap: 32px;
  width: 463px;
  padding: 32px;
  background-color: var(--color-background-form);
  border-radius: 8px;
}

.regform-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  align-self: stretch;
}
.valid {
  color: green;
}
.invalid {
  color: red;
}
</style>
