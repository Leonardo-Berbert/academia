<template>
  <div>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
        <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.2/css/all.min.css" integrity="sha512-SnH5WK+bZxgPHs44uWIX+LLJAJ9/2PkPKZ5QiAj6Ta86w+fsb2TkcmfRyVX3pBnMFcV7oQPJkl9QevSCWr3W6A==" crossorigin="anonymous" referrerpolicy="no-referrer" />
  <div class="border-t-2 border-gray-800 bg-gray-900 text-white p-2.5 text-left flex justify-between items-center"> 
    <i class="fa-solid fa-users fa-xl" style="color: #f5f9ff;"></i>
    <p class="text-lg ml-20">Sistema de Ficha de Treino > Alunos</p>
    <a href="/menu" class="text-white hover:text-slate-400">VOLTAR AO HOME</a>
</div>
    <div class="w-full mx-auto p-6 bg-slate-300 rounded shadow-md border-black">
      <UForm :validate="validate" :state="state" class="space-y-4 " @submit="onSubmit">
        <UFormGroup label="Aluno" name="aluno" class="mb-4 border-black">
          <UInput v-model="state.aluno" class="w-full" placeholder="Nome do aluno" />
        </UFormGroup>
        <UFormGroup label="CPF" name="CPF" class="mb-4">
          <UInput v-model="state.CPF" @input="formatarCPF" class="w-full" placeholder="CPF do aluno" />
        </UFormGroup>
        <UFormGroup label="Telefone" name="telefone" class="mb-4">
          <UInput v-model="state.telefone" @input="formatarTelefone" class="w-full" placeholder="Telefone do aluno" />
        </UFormGroup>
        <UFormGroup label="Data de Nascimento" name="datanasc" class="mb-4">
          <UInput v-model="state.datanasc" @input="formatarDataNascimento" class="w-full" placeholder="Data de nascimento do aluno" />
        </UFormGroup>
        <UFormGroup label="E-mail" name="email" class="mb-4">
          <UInput v-model="state.email" class="w-full" placeholder="E-mail do aluno" />
        </UFormGroup>
        <div class="flex justify-center">
          <UButton type="submit" :disabled="!validaform()" class="hover:bg-slate-950 bg-slate-800 text-white w-96 py-2 px-5 m-6 p-2 rounded-lg text-base flex justify-center items-center">
            Cadastrar
          </UButton>
        </div>
      </UForm>

      <div v-for="(aluno, index) in aluno" :key="index" class="bg-gray-100 rounded p-4 mt-4">
        <div class="mb-2"><strong>Nome:</strong> {{ aluno.aluno }}</div>
        <div class="mb-2"><strong>CPF:</strong> {{ aluno.CPF }}</div>
        <div class="mb-2"><strong>Telefone:</strong> {{ aluno.telefone }}</div>
        <div class="mb-2"><strong>Data de Nascimento:</strong> {{ aluno.datanasc }}</div>
        <div class="mb-2"><strong>E-mail:</strong> {{ aluno.email }}</div>
        <div class="flex gap-2">
          <UButton @click="preencherCampos(aluno)" class="bg-slate-950 hover:bg-slate-800 text-white py-2 tracking-wider px-4 rounded font-sans">
            Preencher Campos
          </UButton>
          <UButton @click="editar(index, aluno)" class="bg-green-500 hover:bg-green-600 text-white py-2 px-4 rounded">
            Editar
          </UButton>
          <UButton @click="remove(aluno.id)" class="bg-red-500 hover:bg-red-600 text-white py-1 px-4 rounded">
            Remover
          </UButton>
        </div>
      </div>
    </div>
  </div>
</template>


<script setup lang="ts">
import data from '../db/db.json'
import type { FormError, FormSubmitEvent } from '#ui/types'
import { reactive } from 'vue'

const aluno = data.alunos
const apiUrl = "http://localhost:8000"

const state = reactive({
  aluno: '',
  CPF: '',
  telefone: '',
  datanasc: '',
  email: ''
})

const validate = (state: any): FormError[] => {
  const errors = []
  if (!state.aluno) {
    errors.push({ path: 'aluno', message: 'Campo obrigatório' })
  }
  if (!state.CPF || state.CPF.length !== 14) {
    errors.push({ path: 'CPF', message: 'CPF deve conter 11 números' })
  }

  if (!state.telefone || state.telefone.length !== 15) {
    errors.push({ path: 'telefone', message: 'Telefone deve conter 11 números' })
  }
  if (!state.datanasc || state.datanasc.length !== 10) {
    errors.push({ path: 'datanasc', message: 'Data de nascimento deve conter 8 números' })
  }
  if (!state.email) {
    errors.push({ path: 'email', message: 'Campo obrigatório' })
  } else {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
    const allowedDomains = ['hotmail.com', 'gmail.com']
    const [, domain] = state.email.match(/@(.+)$/) || []
    if (!emailRegex.test(state.email) || !allowedDomains.includes(domain)) {
      errors.push({ path: 'email', message: 'E-mail inválido ou não permitido' })
    }
  }
  return errors
}
const validaform = () => {
  const errors = validate(state)
  return errors.length === 0
}

async function onSubmit(event: FormSubmitEvent<any>) {

  console.log(event)

  const dataJson = JSON.stringify(state)

  const req = await fetch("http://localhost:8000/alunos", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: dataJson
  })
  state.aluno = ''
  state.CPF = ''
  state.datanasc = ''
  state.telefone = ''
  state.email = ''
}
const nomealert = {
  aluno: 'Aluno',
  CPF: 'CPF',
  telefone: 'Telefone',
  datanasc: 'Data de nascimento',
  email: 'Email'
}

function editar(index: number, aluno: any) {
  const camposObrigatorios = ['aluno', 'CPF', 'telefone', 'datanasc', 'email']

  
  const campoVazio = camposObrigatorios.find(campo => !state[campo])

  if (campoVazio) {
    
    const nomeCampoVazio = nomealert[campoVazio] || campoVazio
    console.log(`Por favor, preencha o campo ${nomeCampoVazio}`)
    alert(`Por favor, preencha o campo ${nomeCampoVazio}`)
    return; 
  }
 
  if (aluno.editavel) {
    fetch(`${apiUrl}/alunos/${aluno.id}`, {
      method: 'PUT',
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ aluno: state.aluno, CPF: state.CPF, datanasc: state.datanasc, telefone: state.telefone, email: state.email })
    }).then(() => {
      console.log('Aluno editado');
    }).catch(err => {
      console.error(err);
    });
  } else {
    aluno.editavel = true;
  }
}
function preencherCampos(aluno: any) {
  state.aluno = aluno.aluno;
  state.CPF = aluno.CPF;
  state.telefone = aluno.telefone;
  state.datanasc = aluno.datanasc;
  state.email = aluno.email;
}
async function remove(id: number) {
  try {
    const req = await fetch(`${apiUrl}/alunos/${id}`, {
      method: 'DELETE'
    })
    console.log('Aluno removido');

  } catch (err) {
    console.error(err)
  }
}
function formatarCPF(event) {
  let cpf = event.target.value.replace(/\D/g, '');
  if (cpf.length <= 11) {
    cpf = cpf.replace(/(\d{3})(\d)/, '$1.$2');
    cpf = cpf.replace(/(\d{3})(\d)/, '$1.$2');
    cpf = cpf.replace(/(\d{3})(\d{1,2})$/, '$1-$2');
    event.target.value = cpf;
  }
}

function formatarTelefone(event) {
  let telefone = event.target.value.replace(/\D/g, '');
  if (telefone.length <= 11) {
    telefone = telefone.replace(/(\d{2})(\d)/, '($1) $2');
    telefone = telefone.replace(/(\d{5})(\d)/, '$1-$2');
    event.target.value = telefone;
  }
}

function formatarDataNascimento(event) {
  let data = event.target.value.replace(/\D/g, '');
  if (data.length <= 8) {
    data = data.replace(/(\d{2})(\d)/, '$1/$2');
    data = data.replace(/(\d{2})(\d)/, '$1/$2');
    event.target.value = data;
  }
}

</script>

<style>
.disabled\:bg-primary-500:disabled {
    --tw-bg-opacity: 1;
    background-color: #020617

}
.border-0 {
    border-width: 1px;
}
</style>
