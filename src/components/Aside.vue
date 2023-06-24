<template>
  <aside>
    <header>
      <RouterLink to="/"> <img src="../assets/logo.svg" alt="esigno logo" /></RouterLink>
    </header>
    <div class="headline-wrapper">
      <h1>Digitální kancelář</h1>
      <h3>Šetří Váš čas i peníze</h3>
    </div>
    <footer>
      <div class="icons-wrapper">
        <img src="./icons/gdpr.png" alt="gdpr logo" />
        <img src="./icons/edias.png" alt="gdpr logo" />
        <img src="./icons/iso_9001.png" alt="gdpr logo" />
        <img src="./icons/iso_27001.png" alt="gdpr logo" />
      </div>
      <p>Copyright © NUBEO Technologies, s.r.o.</p>
    </footer>
  </aside>
</template>

<script setup>
import { RouterLink } from 'vue-router'
</script>

<style scoped>
aside {
  height: 100%;
  width: 100%;
  padding: 32px 0 32px 150px;
  background: linear-gradient(45deg, #545ce4 0%, #e698f8 0.01%, #499af7 100%),
    url('../assets/bg_gradient.png'), lightgray 50% / cover no-repeat;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.headline-wrapper {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

h1 {
  font-size: 40px;
  font-weight: 700;
  color: var(--text-headings);
}

h3 {
  font-size: 24px;
  color: var(--text-headings);
}

.icons-wrapper {
  display: flex;
  gap: 13px;
  height: 50px;
  width: 288px;
  margin-bottom: 24px;
}
</style>
