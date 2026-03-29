<template>
  <div class="app">
    <nav>
      <button @click="page = 'home'" :disabled="!isLoggedIn">Главная</button>
      <button @click="page = 'login'">Вход</button>
      <button @click="page = 'register'">Регистрация</button>
      <button v-if="isLoggedIn" @click="logout">Выйти</button>
    </nav>

    <div class="content">
      <HomePage v-if="page === 'home'" />
      <LoginPage v-else-if="page === 'login'" @login-success="handleLogin" />
      <RegisterPage v-else-if="page === 'register'" @registered="page = 'login'" />
    </div>

    <footer>
      <p>© 2025 Мой Vue проект | Все права защищены</p>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, provide } from 'vue'
import HomePage from './components/HomePage.vue'
import LoginPage from './components/LoginPage.vue'
import RegisterPage from './components/RegisterPage.vue'

// Тип пользователя
interface User {
  name: string
  email: string
  password: string
}

const page = ref<'home' | 'login' | 'register'>('register')
const isLoggedIn = ref(false)
const users = ref<User[]>([])

// Загрузка сохранённых пользователей и авторизации
onMounted(() => {
  const savedUsers = localStorage.getItem('users')
  if (savedUsers) users.value = JSON.parse(savedUsers)

  const loggedInEmail = localStorage.getItem('loggedInEmail')
  if (loggedInEmail && users.value.some(u => u.email === loggedInEmail)) {
    isLoggedIn.value = true
    page.value = 'home'
  }
})

// Сохранение пользователей в localStorage
function saveUsers() {
  localStorage.setItem('users', JSON.stringify(users.value))
}

// Регистрация нового пользователя
function registerUser(user: User) {
  if (users.value.some(u => u.email === user.email)) {
    throw new Error('Пользователь с таким email уже существует')
  }
  users.value.push(user)
  saveUsers()
}

// Проверка входа
function loginUser(email: string, password: string): boolean {
  const user = users.value.find(u => u.email === email && u.password === password)
  if (user) {
    localStorage.setItem('loggedInEmail', email)
    return true
  }
  return false
}

// Обработчик успешного входа из LoginPage
function handleLogin(email: string, password: string) {
  if (loginUser(email, password)) {
    isLoggedIn.value = true
    page.value = 'home'
  } else {
    alert('Неверный email или пароль')
  }
}

// Выход
function logout() {
  localStorage.removeItem('loggedInEmail')
  isLoggedIn.value = false
  page.value = 'login'
}

// Предоставляем функцию регистрации дочерним компонентам
provide('registerUser', registerUser)
</script>

<style scoped>
.app {
  max-width: 500px;
  width: 100%;
  margin: 40px auto;
  font-family: sans-serif;
  display: flex;
  flex-direction: column;
  min-height: 400px;
  border: 1px solid #eee;
  border-radius: 8px;
  box-sizing: border-box;
}
nav {
  display: flex;
  gap: 10px;
  margin: 0 20px 20px 20px;
  padding-top: 20px;
  flex-shrink: 0;
}
button {
  padding: 8px 16px;
  cursor: pointer;
}
button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
.content {
  flex: 1;
  padding: 0 20px;
}
footer {
  text-align: center;
  font-size: 12px;
  color: #666;
  padding: 20px;
  border-top: 1px solid #eee;
  margin-top: 20px;
}
</style>