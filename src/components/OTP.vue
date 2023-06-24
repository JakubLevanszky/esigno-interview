<template>
  <div ref="otpCont" class="pin-wrapper">
    <input
      type="text"
      class="digit-box"
      v-for="(el, ind) in digits"
      :key="el + ind"
      v-model="digits[ind]"
      :autofocus="ind === 0"
      maxlength="1"
      @keydown="handleKeyDown($event, ind)"
    />
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'

const props = defineProps({
  default: String,

  digitCount: {
    type: Number,
    required: true
  }
})

const digits = reactive([])

if (props.default && props.default.length === props.digitCount) {
  for (let i = 0; i < props.digitCount; i++) {
    digits[i] = props.default.charAt(i)
  }
} else {
  for (let i = 0; i < props.digitCount; i++) {
    digits[i] = null
  }
}

const otpCont = ref(null)

const emit = defineEmits(['update:otp'])

const isDigitsFull = function () {
  for (const elem of digits) {
    if (elem == null || elem == undefined) {
      return false
    }
  }

  return true
}

const handleKeyDown = function (event, index) {
  if (event.key !== 'Tab' && event.key !== 'ArrowRight' && event.key !== 'ArrowLeft') {
    event.preventDefault()
  }

  if (event.key === 'Backspace') {
    digits[index] = null

    if (index != 0) {
      otpCont.value.children[index - 1].focus()
    }

    return
  }

  if (new RegExp('^([0-9])$').test(event.key)) {
    digits[index] = event.key

    if (index != props.digitCount - 1) {
      otpCont.value.children[index + 1].focus()
    }

    if (isDigitsFull()) {
      emit('update:otp', digits.join(''))
    }
  }
}
</script>

<style scoped>
.pin-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 7px;
}

.digit-box {
  height: 72px;
  width: 56px;
  border: 1px solid var(--input-border);
  text-align: center;
  border-radius: 10px;
  font-size: 32px;
  font-weight: 700;
}
</style>
