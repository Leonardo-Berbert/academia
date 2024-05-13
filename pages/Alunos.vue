<script setup lang="ts">
import data from '../db/db.json'
import type { FormError ,FormSubmitEvent} from '#ui/types'
import { reactive } from 'vue'

const aluno = data.aluno
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

  const req = await fetch("http://localhost:8000/aluno", {
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

function editar(index: number, aluno: any) {
  if (aluno.editavel) {
    fetch(`${apiUrl}/aluno/${aluno.id}`,
      {
        method: 'PUT',
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ nome: state.nome, CPF: state.CPF, datanasc: state.datanasc, telefone: state.telefone, email: state.email })

      }).then(() => {
        console.log('Aluno editado');
      }).catch(err => {
        console.error(err)
      });

  }
  else {

    aluno.editavel = true;
  }
}
function preencherCampos(aluno: any) {
  state.nome = aluno.nome;
  state.CPF = aluno.CPF;
  state.telefone = aluno.telefone;
  state.datanasc = aluno.datanasc;
  state.email = aluno.email;
}
async function remove(id: number) {
  try {
    const req = await fetch(`${apiUrl}/aluno/${id}`, {
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

<template>
  <UForm :validate="validate" :state="state" class="space-y-4" @submit="onSubmit">
    <UFormGroup label="aluno" name="aluno">
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



  <div v-for="(aluno, index) in aluno" :key="index">
    <div>nome:{{ aluno.nome }}</div>
    <div>CPF:{{ aluno.CPF }}</div>
    <div>TELEFONE:{{ aluno.telefone }}</div>
    <div>DATA DE NASCIMENTO:{{ aluno.datanasc }}</div>
    <div>EMAIL:{{ aluno.email }}</div>
    <UButton @click="preencherCampos(aluno)">Preencher Campos</UButton>
    <UButton @click="editar(index, aluno)">Editar</UButton>
    <UButton @click="remove(aluno.id)">Remover</UButton>
    <hr>
  </div>



</template>
