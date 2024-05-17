<script setup lang="ts">
import data from '../db/db.json'
import type { FormError ,FormSubmitEvent} from '#ui/types'
import { reactive } from 'vue'

const instrutor = data.instrutores
const apiUrl = "http://localhost:8000"

const state = reactive({
  instrutor: '',
  CPF: '',
  telefone: '',
  datanasc: '',
  email: ''
})

const validate = (state: any): FormError[] => {
  const errors = []
  if (!state.instrutor) {
    errors.push({ path: 'instrutor', message: 'campo obrigatório' })
  }
  if (!state.CPF || state.CPF.length !== 14) {
    errors.push({ path: 'CPF', message: 'CPF deve conter 11 números' })
  }

  if (!state.telefone || state.telefone.length !== 15) {
    errors.push({ path: 'telefone', message: 'telefone deve conter 11 números' })
  }
  if (!state.datanasc || state.datanasc.length !== 10) {
    errors.push({ path: 'datanasc', message: 'data de nascimento deve conter 8 números' })
  }
  if (!state.email) {
    errors.push({ path: 'email', message: 'campo obrigatório' })
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

  const req = await fetch("http://localhost:8000/instrutores", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: dataJson
  })
  state.instrutor = '',
    state.CPF = '',
    state.datanasc = '',
    state.telefone = '',
    state.email = ''
}
const nomealert = {
  instrutor: 'Instrutor',
  CPF: 'CPF',
  telefone: 'Telefone',
  datanasc: 'Data de nascimento',
  email: 'Email'
}
function editar(index: number, instrutor: any) {
  const camposObrigatorios = ['instrutor', 'CPF', 'telefone', 'datanasc', 'email']

  
  const campoVazio = camposObrigatorios.find(campo => !state[campo])

  if (campoVazio) {
    
    const nomeCampoVazio = nomealert[campoVazio] || campoVazio
    console.log(`Por favor, preencha o campo ${nomeCampoVazio}`)
    alert(`Por favor, preencha o campo ${nomeCampoVazio}`)
    return; 
  }
  if (instrutor.editavel) {
    fetch(`${apiUrl}/instrutores/${instrutor.id}`,
      {
        method: 'PUT',
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ instrutor: state.instrutor, CPF: state.CPF, datanasc: state.datanasc, telefone: state.telefone, email: state.email })

      }).then(() => {
        console.log('instrutor editado');
      }).catch(err => {
        console.error(err)
      });

  }
  else {

    instrutor.editavel = true;
  }
}
function preencherCampos(instrutor: any) {
  state.instrutor = instrutor.instrutor;
  state.CPF = instrutor.CPF;
  state.telefone = instrutor.telefone;
  state.datanasc = instrutor.datanasc;
  state.email = instrutor.email;
}
async function remove(id: number) {
  try {
    const req = await fetch(`${apiUrl}/instrutores/${id}`, {
      method: 'DELETE'
    })
    console.log('instrutor removido');

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
<template>
  <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
        <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.2/css/all.min.css" integrity="sha512-SnH5WK+bZxgPHs44uWIX+LLJAJ9/2PkPKZ5QiAj6Ta86w+fsb2TkcmfRyVX3pBnMFcV7oQPJkl9QevSCWr3W6A==" crossorigin="anonymous" referrerpolicy="no-referrer" />
  <div class="border-t-2 border-gray-800 bg-gray-900 text-white p-2.5 text-left flex justify-between items-center"> 
    <i class="fa-solid fa-user fa-xl" style="color: #fafcff;"></i>
    <p class="text-lg ml-24">Sistema Ficha de Treino > Instrutores</p>
    <a href="/menu" class="text-white hover:text-slate-400">VOLTAR AO HOME</a>
</div>
  <div class="w-full mx-auto p-5 bg-slate-300 rounded shadow-md border-gray-800">
    <UForm :validate="validate" :state="state" class="space-y-4" @submit="onSubmit">
      <UFormGroup label="Instrutor" name="instrutor" class="mb-4 border-gray-800 text-black">
        <UInput v-model="state.instrutor" class="w-full" placeholder="Nome do instrutor" />
      </UFormGroup>
      <UFormGroup label="CPF" name="CPF" class="mb-4">
        <UInput v-model="state.CPF" @input="formatarCPF" class="w-full" placeholder="CPF do Instrutor" />
      </UFormGroup>
      <UFormGroup label="Telefone" name="telefone" class="mb-4">
        <UInput v-model="state.telefone" @input="formatarTelefone" class="w-full" placeholder="Telefone do Instrutor" />
      </UFormGroup>
      <UFormGroup label="Data de Nascimento" name="datanasc" class="mb-4">
        <UInput v-model="state.datanasc" @input="formatarDataNascimento" class="w-full placeholder-slate-950" placeholder="Data de nascimento do instrutor"/>
      </UFormGroup>
      <UFormGroup label="E-mail" name="email" class="mb-4">
        <UInput v-model="state.email" class="w-full text-black" placeholder="E-mail do instrutor" />
      </UFormGroup>
      <div class="flex justify-center">
        <UButton type="submit" :disabled="!validaform()" class="hover:bg-slate-950 bg-slate-800 text-white w-96 py-2 px-5 m-6 p-2 rounded-lg text-base flex justify-center items-center">
            Cadastrar
          </UButton>
      </div>
    </UForm>

    <div v-for="(instrutor, index) in instrutor" :key="index" class="bg-gray-100 w-full rounded p-1 mt-4">
      <div class="mb-2 justify-center items font-sans"><strong>Nome:</strong> {{ instrutor.instrutor}}</div>
      <div class="mb-2 justify-center items font-sans"><strong>CPF:</strong> {{ instrutor.CPF }}</div>
      <div class="mb-2 justify-center items font-sans"><strong>Telefone:</strong> {{ instrutor.telefone }}</div>
      <div class="mb-2 justify-center items font-sans"><strong>Data de Nascimento:</strong> {{ instrutor.datanasc }}</div>
      <div class="mb-2 justify-center items font-sans"><strong>E-mail:</strong> {{ instrutor.email }}</div>
      <div class="flex gap-2">
        <UButton @click="preencherCampos(instrutor)" class="bg-slate-950 hover:bg-slate-800 text-white py-2 tracking-wider px-4 rounded font-sans">
          Preencher Campos</UButton>
        <UButton @click="editar(index, instrutor)" class="bg-green-500 hover:bg-green-600 text-white py-2 px-4 rounded">
          Editar
        </UButton>
        <UButton @click="remove(instrutor.id)" class="bg-red-500 hover:bg-red-600 text-white py-1 px-4 rounded">
          Remover
        </UButton>
      </div>
    </div>
  </div>
</template>
<style>
.disabled\:bg-primary-500:disabled {
    --tw-bg-opacity: 1;
    background-color: #020617

}
.border-0 {
    border-width: 1px;
}


</style>
