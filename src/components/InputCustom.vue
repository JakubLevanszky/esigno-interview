<template>
  <div class="input-wrapper">
    <label class="label" v-if="label"> {{ label }}</label>
    <input
      type="text"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      v-bind="$attrs"
    />
    <!-- <img src="/icons/eye_closed_bold.svg" height="30" width="30" class="icon" /> -->
  </div>
</template>

<script setup>
defineProps({
  label: {
    type: [Boolean, String]
  },
  modelValue: {
    type: String,
    default: ''
  }
})
</script>

<style scoped>
@import '@/assets/base.css';
.input-wrapper {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.label {
  font-size: 12px;
}

input {
  height: 36px;
  border: 1px solid var(--input-border); /* move it to the .input-wrapper  */
  border-radius: 4px;
}
/* .icon {
  color: black;
} */
</style>
