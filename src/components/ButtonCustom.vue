<template>
  <button :class="buttonClass" @click="handleClick">
    <slot></slot>
  </button>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  primary: {
    type: Boolean,
    default: false
  },
  disabled: {
    type: Boolean,
    default: false
  }
})

const buttonClass = computed(() => {
  return `button ${props.primary ? 'button-primary' : ''} ${
    props.disabled ? 'button-disabled' : ''
  }`
})

const handleClick = () => {
  if (!props.disabled) {
    this.emit('click')
  }
}
</script>

<style scoped>
.button {
  width: 399px;
  border-radius: 4px;
  background: var(--button-primary);
  padding: 17px 0px;
  border: none;
  color: var(--text-white);
  font-size: 14px;
  font-family: Inter;
  font-weight: 600;
  cursor: pointer;
}

.button:hover {
  background: var(--button-hover);
}

.button-disabled {
  background: var(--button-primary);
  pointer-events: none;
  opacity: 0.5;
}
</style>
