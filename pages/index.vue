<template>
  <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
        <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.2/css/all.min.css" integrity="sha512-SnH5WK+bZxgPHs44uWIX+LLJAJ9/2PkPKZ5QiAj6Ta86w+fsb2TkcmfRyVX3pBnMFcV7oQPJkl9QevSCWr3W6A==" crossorigin="anonymous" referrerpolicy="no-referrer" />
  <div class="bg-gradient-to-r from-slate-950 via-neutral-500 to-slate-950 flex justify-center items-center h-screen">
    <div class="bg-slate-950 rounded-xl border-zinc-500  w-120 ml-30 mt-10 p-5">
    <div class="flex justify-center items-center ">
          <div class="w-20 h-20 flex justify-center items-center bg-sky-950 rounded-full mt-50 left-3 top-1/2 transform -translate-y-1/2">
            <i class="fa-solid fa-user fa-3x " style="color: #000000;"></i>
          </div>
        </div>
      
        <form @submit.prevent="login">
          <div class="mb-8 relative">
                        <i class=" "></i>
                        <div class="mb-6">
                          <i class="fa-solid fa-user fa-lg" style="color: #082f49;"></i>
                          <input class=" m-2 bg-transparen border-button border-solid outline-none leading-tight focus:outline-none focus:shadow-outline rounded p-1.5 ml-3.5 h-8 w-80 border bg-none bg-slate-600 text-white" id="usuarios" type="text" placeholder="Usuário" v-model="usuarios">
                        </div>  
          </div>

          <div class="mb-10">
            <i class="fa-solid fa-lock fa-lg" style="color: #082f49;" ></i>
            <input class=" bg-transparen border-button border-solid outline-none leading-tight focus:outline-none focus:shadow-outline rounded p-3 ml-3.5 h-8 w-80 border bg-none bg-slate-600 text-white" id="senhas" type="password" placeholder="Senha"  v-model="senhas" >
          </div>
          <p v-if="error" class="error flex justify-center">{{ error }}</p>
          <div class="flex justify-center">
            <button type="submit" class="w-80 h-11 ml-4 tracking-widest flex justify-center bg-neutral-800 text-white font-serif py-3 px-6 p-1.5 rounded hover:bg-neutral-600">LOGIN</button>
          </div>
          
          <a href="/cadastro" class="text-sky-700 hover:text-sky-900 flex justify-center items-center ml-10 m-5">Criar minha conta</a>
        </form>

    </div>
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
      window.location.href = 'http://localhost:3000/menu'
     
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