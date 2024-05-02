<script setup lang="ts">
import data from '../db/db.json'
import type { FormError, FormSubmitEvent } from '#ui/types'

const aluno = data.aluno
const apiUrl = "http://localhost:8000"

const state = reactive({
  nome: undefined,
  //carga: undefined
})

const validate = (state: any): FormError[] => {
  const errors = []
  //if (!state.carga) errors.push({ path: 'carga', message: 'campo obrigatorio' })
  if (!state.nome) errors.push({ path: 'nome', message: 'campo obrigatorio' })
  return errors
}

async function onSubmit (event: FormSubmitEvent<any>) {
  // Do something with data
  console.log(event.data)
  const dataJson = JSON.stringify(state)

        const req = await fetch("http://localhost:8000/aluno", {
            method: "POST",
            headers: {"Content-Type": "application/json"},
            body: dataJson
        })

        state.nome = undefined
        //state.carga = undefined
      }
  

function editar(index: number, aluno: any) {
  // Se o aluno estiver editável, salva as alterações
  if (aluno.editavel) {
    // Realiza a atualização do aluno na API
    fetch(`${apiUrl}/aluno/${aluno.id}`, {
      method: 'PUT',
      headers: {"Content-Type": "application/json"},
      body: JSON.stringify({ nome: aluno.nome })
    }).then(() => {
      console.log('Aluno editado');
    }).catch(err => {
      console.error(err)
    });
  } else {
    // Se o aluno não estiver editável, habilita a edição
    aluno.editavel = true;
  }
}
async function remove(id: number) {
  try {
    const req = await fetch(`${apiUrl}/aluno/${id}`, {
      method: 'DELETE'
    })
    console.log('Aluno removido');
    // Atualizar a lista de alunos após a remoção
 
  } catch (err) {
    console.error(err)
  }
}

</script>

<template>
    <UForm :validate="validate" :state="state" class="space-y-4" @submit="onSubmit">
        <UFormGroup label="aluno" name="aluno">
            <UInput v-model="state.nome" />
        </UFormGroup>

        

        <UButton type="submit">
            cadastrar
        </UButton>
        <div v-for="(aluno, index) in aluno" :key="index">
          <p v-if="!aluno.editavel">{{ aluno.nome }}</p>
          <UInput v-else v-model="aluno.nome" />
          <UButton @click="editar(index, aluno)">Editar</UButton>
          <UButton @click="remove(aluno.id)">Remover</UButton>
          <hr>
        </div>
  
    </UForm>

    <UTable :rows="aluno" />
</template>
