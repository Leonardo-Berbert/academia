<script setup lang="ts">
import data from '../db/db.json'
import type { FormError ,FormSubmitEvent} from '#ui/types'
import { reactive } from 'vue'

const instrutor = data.instrutor
const apiUrl = "http://localhost:8000"

const state = reactive({
  nome: '',
  CPF: '',
  telefone: '',
  datanasc: '',
  email: ''
})

const validate = (state: any): FormError[] => {
  const errors = []
  if (!state.nome) {
    errors.push({ path: 'nome', message: 'campo obrigatório' })
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

  const req = await fetch("http://localhost:8000/instrutor", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: dataJson
  })
  state.nome = '',
    state.CPF = '',
    state.datanasc = '',
    state.telefone = '',
    state.email = ''
}

function editar(index: number, instrutor: any) {
  if (instrutor.editavel) {
    fetch(`${apiUrl}/instrutor/${instrutor.id}`,
      {
        method: 'PUT',
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ nome: state.nome, CPF: state.CPF, datanasc: state.datanasc, telefone: state.telefone, email: state.email })

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
  state.nome = instrutor.nome;
  state.CPF = instrutor.CPF;
  state.telefone = instrutor.telefone;
  state.datanasc = instrutor.datanasc;
  state.email = instrutor.email;
}
async function remove(id: number) {
  try {
    const req = await fetch(`${apiUrl}/instrutor/${id}`, {
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
  <UForm :validate="validate" :state="state" class="space-y-4" @submit="onSubmit">
    <UFormGroup label="instrutor" name="instrutor">
      <UInput v-model="state.nome" />
    </UFormGroup>
    <UFormGroup label="CPF" name="CPF">
      <UInput v-model="state.CPF" @input="formatarCPF" />
    </UFormGroup>
    <UFormGroup label="telefone" name="telefone">
      <UInput v-model="state.telefone" @input="formatarTelefone" />
    </UFormGroup>
    <UFormGroup label="datanasc" name="datanasc">
      <UInput v-model="state.datanasc" @input="formatarDataNascimento" />
    </UFormGroup>
    <UFormGroup label="email" name="email">
      <UInput v-model="state.email" />
    </UFormGroup>
    <UButton type="submit" :disabled="!validaform()">
      Cadastrar
    </UButton>
  </UForm>



  <div v-for="(instrutor, index) in instrutor" :key="index">
    <div>nome:{{ instrutor.nome }}</div>
    <div>CPF:{{ instrutor.CPF }}</div>
    <div>TELEFONE:{{ instrutor.telefone }}</div>
    <div>DATA DE NASCIMENTO:{{ instrutor.datanasc }}</div>
    <div>EMAIL:{{ instrutor.email }}</div>
    <UButton @click="preencherCampos(instrutor)">Preencher Campos</UButton>
    <UButton @click="editar(index, instrutor)">Editar</UButton>
    <UButton @click="remove(instrutor.id)">Remover</UButton>
    <hr>
  </div>



</template>
