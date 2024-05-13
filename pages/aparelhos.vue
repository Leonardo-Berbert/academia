<script setup lang="ts">
import data from '../db/db.json'
import type { FormError, FormSubmitEvent } from '#ui/types'

const aparelhos = data.aparelhos
const apiUrl = "http://localhost:8000"


const state = reactive({
  nome: undefined,
  cargaM: undefined
})

const validate = (state: any): FormError[] => {
  const errors = []
  if (!state.cargaM) errors.push({ path: 'cargaM', message: 'campo obrigatorio' })
  if (!state.nome) errors.push({ path: 'nome', message: 'campo obrigatorio' })
  return errors
}

async function onSubmit (event: FormSubmitEvent<any>) {
  // Do something with data
  console.log(event.data)
  const dataJson = JSON.stringify(state)

        const req = await fetch("http://localhost:8000/aparelhos", {
            method: "POST",
            headers: {"Content-Type": "application/json"},
            body: dataJson
        })

        state.nome = undefined
        state.cargaM = undefined
}
function editar(index: number, aparelhos: any) {
    if (aparelhos.editavel) 
    {
      fetch(`${apiUrl}/aparelhos/${aparelhos.id}`, 
      {
        method: 'PUT',
        headers: {"Content-Type": "application/json"},
        body: JSON.stringify({ nome: aparelhos.nome })
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
   
    aparelhos.editavel = true;
  }
}
async function remove(id: number) 
{
  try {
    const req = await fetch(`${apiUrl}/aparelhos/${id}`, {
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
        <UFormGroup label="Aparelhos" name="aparelhos">
            <UInput v-model="state.nome" />
        </UFormGroup>

        <UFormGroup label="Carga Máxima" name="cargaM">
            <UInput v-model="state.cargaM" type="cargaM" />
        </UFormGroup>

        <UButton type="submit">
            cadastrar
        </UButton>
        <div v-for="(aparelhos, index) in aparelhos" :key="index">
          <p v-if="!aparelhos.editavel">{{ aparelhos.nome }}</p>
          <UInput v-else v-model="aparelhos.nome" />
          <UButton @click="editar(index, aparelhos)">Editar</UButton>
          <UButton @click="remove(aparelhos.id)">Remover</UButton>
          <hr>
        </div>
    </UForm>

    <UTable :rows="aparelhos" />
</template>

