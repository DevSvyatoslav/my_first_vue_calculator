<template>
  <div>
    <h2>Калькулятор</h2>
    <input type="number" v-model.number="a" placeholder="Число 1" />
    <select v-model="op">
      <option value="+">+</option>
      <option value="-">-</option>
      <option value="*">*</option>
      <option value="/">/</option>
    </select>
    <input type="number" v-model.number="b" placeholder="Число 2" />
    <button @click="calc">=</button>
    <p>Результат: {{ result }}</p>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const a = ref(0)
const b = ref(0)
const op = ref('+')
const result = ref(0)

function calc() {
  if (op.value === '+') result.value = a.value + b.value
  if (op.value === '-') result.value = a.value - b.value
  if (op.value === '*') result.value = a.value * b.value
  if (op.value === '/') result.value = a.value / b.value
}
</script>

<style scoped>
input, select, button {
  margin: 5px;
  padding: 5px;
}
</style>