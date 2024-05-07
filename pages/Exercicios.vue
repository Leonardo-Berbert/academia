<script setup lang="ts">
import data from '../db/db.json'
import type { FormError, FormSubmitEvent } from '#ui/types'

const exercicio = data.exercicio
const apiUrl = "http://localhost:8000"


const state = reactive({
  nome: undefined,
  aparelho: undefined
})

const validate = (state: any): FormError[] => {
  const errors = []
  if (!state.aparelho) errors.push({ path: 'aparelho', message: 'campo obrigatorio' })
  if (!state.nome) errors.push({ path: 'nome', message: 'campo obrigatorio' })
  return errors
}

async function onSubmit (event: FormSubmitEvent<any>) {
  // Do something with data
  console.log(event.data)
  const dataJson = JSON.stringify(state)

        const req = await fetch("http://localhost:8000/exercicio", {
            method: "POST",
            headers: {"Content-Type": "application/json"},
            body: dataJson
        })

        state.nome = undefined
        state.aparelho = undefined
}
function editar(index: number, exercicio: any) {
    if (exercicio.editavel) 
    {
      fetch(`${apiUrl}/exercicio/${exercicio.id}`, 
      {
        method: 'PUT',
        headers: {"Content-Type": "application/json"},
        body: JSON.stringify({ nome: exercicio.nome })
      }).then(() => 
      {
        console.log('Aluno editado');
      }).catch(err => 
      {
        console.error(err)
      });
    } 
    else 
  {
   
    exercicio.editavel = true;
  }
}
async function remove(id: number) 
{
  try {
    const req = await fetch(`${apiUrl}/exercicio/${id}`, {
      method: 'DELETE'
    })
    console.log('Aluno removido');
 
  } catch (err) {
    console.error(err)
  }
}
</script>

<template>
    <UForm :validate="validate" :state="state" class="space-y-4" @submit="onSubmit">
        <UFormGroup label="exercicio" name="exercicio">
            <UInput v-model="state.nome" />
        </UFormGroup>

        <UFormGroup label="aparelho" name="aparelho">
            <UInput v-model="state.aparelho" type="aparelho" />
        </UFormGroup>

        <UButton type="submit">
            cadastrar
        </UButton>
        <div v-for="(exercicio, index) in exercicio" :key="index">
          <p v-if="!exercicio.editavel">{{ exercicio.nome }}</p>
          <UInput v-else v-model="exercicio.nome" />
          <UButton @click="editar(index, exercicio)">Editar</UButton>
          <UButton @click="remove(exercicio.id)">Remover</UButton>
          <hr>
        </div>
    </UForm>

    <UTable :rows="exercicio" />
</template>

