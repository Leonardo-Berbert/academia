<template>
  <div>
    <h1>Login</h1>
    <form @submit.prevent="login">
      <div>
        <label for="usuarios">Usuário:</label>
        <input type="text" id="usuarios" v-model="usuarios" />
      </div>
      <div>
        <label for="senhas">Senha:</label>
        <input type="password" id="senhas" v-model="senhas" />
      </div>
      <button type="submit">Entrar</button>
      <p v-if="error" class="error">{{ error }}</p>
    </form>
    <a href="/login">CADASTRE-SE</a>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import data from '../db/db.json'

const usuarios= ref('')
const senhas = ref('')
const error = ref('')

async function login() {
  try {
    const user = data.login.find(user => user.usuarios === usuarios.value && user.senhas === senhas.value)
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
