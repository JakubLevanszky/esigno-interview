<template>
  <div class="input-wrapper">
    <label v-if="label"> {{ label }}</label>
    <input
      type="text"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      v-bind="$attrs"
    />
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

<style lang="scss" scoped></style>
