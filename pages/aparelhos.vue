<template>
  <div>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
        <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.2/css/all.min.css" integrity="sha512-SnH5WK+bZxgPHs44uWIX+LLJAJ9/2PkPKZ5QiAj6Ta86w+fsb2TkcmfRyVX3pBnMFcV7oQPJkl9QevSCWr3W6A==" crossorigin="anonymous" referrerpolicy="no-referrer" />
  <div class="border-t-2 border-gray-800 bg-gray-900 text-white p-2.5 text-left flex justify-between items-center"> 
    <i class="fa-solid fa-dumbbell fa-xl" style="color: #f5f5f5;"></i>
    <p class="text-lg">Sistema Ficha de Treino > Aparelhos</p>
    <a href="/menu" class="text-white hover:text-slate-400">VOLTAR AO HOME</a>
</div>
    <div class="w-full mx-auto p-5 bg-white rounded shadow-md border-gray-800">
      <UForm :validate="validate" :state="state" class="space-y-4" @submit="onSubmit">
        <UFormGroup label="Aparelho" name="aparelho" class="mb-4 border-black">
          <UInput v-model="state.aparelho" class="w-full" placeholder="aparelho do aparelho" />
        </UFormGroup>
        <UFormGroup label="Carga Máxima" name="cargaM" class="mb-4 border-black">
          <UInput v-model="state.cargaM" class="w-full"  type="number" placeholder="Digite quanto o aparelho suporta" />
        </UFormGroup>


        <div class="flex justify-center">
          <UButton type="submit" :disabled="!validaform()" class="hover:bg-slate-950 bg-slate-800 text-white w-96 py-2 px-5 p-2 rounded-lg text-base flex justify-center items-center">
            Cadastrar
          </UButton>
        </div>

        <div v-for="(aparelho, index) in aparelho" :key="index" class="bg-emerald-50 rounded p-2 mt-4">
          <div class="mb-2"><strong>Aparelho:</strong> {{ aparelho.aparelho }}</div>
          <div class="mb-2"><strong>Carga Máxima:</strong> {{ aparelho.cargaM }}</div>
          <div class="flex gap-2">
            <UButton @click="preencherCampos(aparelho)" class="bg-slate-950 hover:bg-slate-800 text-white py-2 tracking-wider px-4 rounded font-sans">
              Preencher Campos
            </UButton>
            <UButton @click="editar(index, aparelho)" class="bg-green-500 hover:bg-green-600 text-white py-2 px-4 rounded">
              Editar
            </UButton>
            <UButton @click="remove(aparelho.id)" class="bg-red-500 hover:bg-red-600 text-white py-1 px-4 rounded">
              Remover
            </UButton>
          </div>
        </div>
      </UForm>
    </div>
  </div>
</template>

<script setup lang="ts">
import data from '../db/db.json'
import type { FormError, FormSubmitEvent } from '#ui/types'
import { reactive } from 'vue'

const aparelho = data.aparelhos
const apiUrl = "http://localhost:8000"

const state = reactive({
  aparelho: '',
  cargaM: ''
})

const validate = (state: any): FormError[] => {
  const errors = []
  if (!state.aparelho) {
    errors.push({ path: 'aparelho', message: 'Campo obrigatório' })
  }
  if (!state.cargaM) {
    errors.push({ path: 'cargaM', message: 'Campo obrigatório' })
  }
  return errors
}

async function onSubmit (event: FormSubmitEvent<any>) {
  console.log(event.data)
  const dataJson = JSON.stringify(state)
  const req = await fetch("http://localhost:8000/aparelhos", {
    method: "POST",
    headers: {"Content-Type": "application/json"},
    body: dataJson
  })
  state.aparelho = ''
  state.cargaM = ''
}
const nomealert = {
  aparelho: 'Aparelho',
  cargaM: 'Carga Máxima'
}
function editar(index: number, aparelho: any) {
  const camposObrigatorios = ['aparelho', 'cargaM',]

  const campoVazio = camposObrigatorios.find(campo => !state[campo])

  if (campoVazio) {
    
    const nomeCampoVazio = nomealert[campoVazio] || campoVazio
    console.log(`Por favor, preencha o campo ${nomeCampoVazio}`)
    alert(`Por favor, preencha o campo ${nomeCampoVazio}`)
    return; 
  }
  if (aparelho.editavel) {
    fetch(`${apiUrl}/aparelhos/${aparelho.id}`, {
      method: 'PUT',
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ aparelho: state.aparelho, cargaM: state.cargaM })
    }).then(() => {
      console.log('aparelho editado');
    }).catch(err => {
      console.error(err)
    });
  } else {
    aparelho.editavel = true;
  }
}

async function remove(id: number) {
  try {
    const req = await fetch(`${apiUrl}/aparelhos/${id}`, {
      method: 'DELETE'
    })
    console.log('aparelho removido');
  } catch (err) {
    console.error(err)
  }
}

function preencherCampos(aparelho: any) {
  state.aparelho = aparelho.aparelho;
  state.cargaM = aparelho.cargaM;
}

const validaform = () => {
  const errors = validate(state)
  return errors.length === 0
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
