<template>
  <div>
    <h1>Login</h1>
    <form @submit.prevent="login">
      <div>
        <label for="username">Usuário:</label>
        <input type="text" id="username" v-model="username" />
      </div>
      <div>
        <label for="password">Senha:</label>
        <input type="password" id="password" v-model="password" />
      </div>
      <button type="submit">Entrar</button>
      <p v-if="error" class="error">{{ error }}</p>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import data from '../db/db.json'

const username = ref('')
const password = ref('')
const error = ref('')

async function login() {
  try {
    const user = data.login.find(user => user.username === username.value && user.password === password.value)
    if (user) {
      
      console.log('Credenciais válidas')
      window.location.href = 'http://localhost:3000/Alunos'
     
    } else {
     
      error.value = 'Credenciais inválidas'
    }
  } catch (error) {
    console.error('Erro ao processar login:', error)
    error.value = 'Erro ao processar login'
  }
}
</script>

<style scoped>
.error {
  color: red;
}
</style>
