<script setup></script>

<template>
  <footer class="footer-wrapper">
    <div class="links-wrapper">
      <a href="#">Prohlášení o GDPR </a>
      <div class="vl"></div>
      <a href="#">Veřejné obchodní podmínky </a>
    </div>
    <div class="ellipse-wrapper">
      <div class="circle"></div>
      <span>Všechny systémy v provozu</span>
    </div>
  </footer>
</template>

<style scoped>
.footer-wrapper {
  display: flex;
  align-items: center;
  gap: 44px;
  position: absolute;
  bottom: 0;
  margin-bottom: 32px;
}

.links-wrapper {
  display: flex;
  gap: 8px;
}
.vl {
  width: 1px;
  height: 13px;
  background-color: var(--light-gray);
}

.circle {
  width: 10px;
  height: 10px;
  border-radius: 10px;
  background: var(--text-positive);
}

.ellipse-wrapper {
  border-radius: 45px;
  color: var(--text-positive);
  padding: 10px 15px;
  background: var(--color-background-form);
  display: inline-flex;
  align-items: center;
  gap: 4px;
}

a {
  font-size: 10px;
}
</style>
