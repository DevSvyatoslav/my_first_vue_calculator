<template>
  <div>
    <h2>Регистрация</h2>
    <form @submit.prevent="register">
      <div>
        <label>Имя:</label>
        <input type="text" v-model="name" required />
      </div>
      <div>
        <label>Email:</label>
        <input type="email" v-model="email" required />
      </div>
      <div>
        <label>Пароль:</label>
        <input type="password" v-model="password" required />
      </div>
      <div>
        <label>Подтвердите:</label>
        <input type="password" v-model="confirm" required />
      </div>
      <button type="submit">Зарегистрироваться</button>
      <p v-if="passError" class="error">Пароли не совпадают</p>
      <p v-if="regError" class="error">{{ regError }}</p>
    </form>
  </div>
</template>

<script setup lang="ts">
import { ref, inject } from 'vue'

const emit = defineEmits<{ registered: [] }>()

// Получаем функцию регистрации из родителя
const registerUser = inject<(user: { name: string; email: string; password: string }) => void>('registerUser')

const name = ref('')
const email = ref('')
const password = ref('')
const confirm = ref('')
const passError = ref(false)
const regError = ref('')

async function register() {
  passError.value = false
  regError.value = ''

  if (password.value !== confirm.value) {
    passError.value = true
    return
  }

  try {
    if (registerUser) {
      registerUser({
        name: name.value,
        email: email.value,
        password: password.value
      })
    }

    // Имитация отправки кода (можно убрать)
    const fakeCode = Math.floor(100000 + Math.random() * 900000)
    console.log(`📧 Код подтверждения для ${email.value}: ${fakeCode}`)
    alert(`Учебный режим: ваш код ${fakeCode}. В реальном проекте он пришёл бы на email.`)

    emit('registered')
  } catch (err: any) {
    regError.value = err.message
  }
}
</script>

<style scoped>
div {
  margin-bottom: 10px;
}
label {
  display: inline-block;
  width: 100px;
}
.error {
  color: red;
  font-size: 12px;
}
</style>