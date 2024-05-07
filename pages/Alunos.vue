<script setup lang="ts">
import data from '../db/db.json'
import type { FormError, FormSubmitEvent } from '#ui/types'

const aluno = data.aluno
const apiUrl = "http://localhost:8000"

const state = reactive({
  nome: undefined,
  idade: undefined
})

const validate = (state: any): FormError[] => {
  const errors = []
  if (!state.idade) errors.push({ path: 'idade', message: 'campo obrigatorio' })
  if (!state.nome) errors.push({ path: 'nome', message: 'campo obrigatorio' })
  return errors
}

async function onSubmit (event: FormSubmitEvent<any>) 
{
  
  console.log(event.data)
  const dataJson = JSON.stringify(state)

        const req = await fetch("http://localhost:8000/aluno", {
            method: "POST",
            headers: {"Content-Type": "application/json"},
            body: dataJson
        })

        state.nome = undefined,
        state.idade = undefined
      }
    function editar(index: number, aluno: any) {
    if (aluno.editavel) 
    {
      fetch(`${apiUrl}/aluno/${aluno.id}`, 
      {
        method: 'PUT',
        headers: {"Content-Type": "application/json"},
        body: JSON.stringify({ nome: state.nome, idade: state.idade })
        
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
   
    aluno.editavel = true;
  }
}
async function remove(id: number) 
{
  try {
    const req = await fetch(`${apiUrl}/aluno/${id}`, {
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
        <UFormGroup label="aluno" name="aluno">
            <UInput v-model="state.nome" />
        </UFormGroup>
        <UFormGroup label="idade" name="idade">
          <UInput v-model="state.idade" />
        </UFormGroup>
       


        

        <UButton type="submit">
            cadastrar
        </UButton>
       
  
    </UForm>

    <UTable :rows="aluno" />
    <div v-for="(aluno, index) in aluno" :key="index">
        <div v-if="!aluno.editavel">{{ aluno.nome }}</div>
          <UInput v-else v-model="aluno.nome" />
          <UButton @click="editar(index, aluno)">Editar</UButton>
          <UButton @click="remove(aluno.id)">Remover</UButton>
          <hr>
    </div>
       
    
</template>
