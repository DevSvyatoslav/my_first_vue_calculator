<template>
  <div>
    <h2>Вход</h2>
    <form @submit.prevent="login">
      <div>
        <label>Email:</label>
        <input type="email" v-model="email" required />
      </div>
      <div>
        <label>Пароль:</label>
        <input type="password" v-model="password" required />
      </div>
      <button type="submit">Войти</button>
    </form>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const emit = defineEmits<{
  'login-success': [email: string, password: string]
}>()

const email = ref('')
const password = ref('')

function login() {
  emit('login-success', email.value, password.value)
}
</script>

<style scoped>
div {
  margin-bottom: 10px;
}
label {
  display: inline-block;
  width: 80px;
}
</style>