<script setup lang="ts">
import data from '../db/db.json'
import type { FormError, FormSubmitEvent } from '#ui/types'

const equipamento = data.equipamento
const apiUrl = "http://localhost:8000"


const state = reactive({
  nome: undefined,
  carga: undefined
})

const validate = (state: any): FormError[] => {
  const errors = []
  if (!state.carga) errors.push({ path: 'carga', message: 'campo obrigatorio' })
  if (!state.nome) errors.push({ path: 'nome', message: 'campo obrigatorio' })
  return errors
}

async function onSubmit (event: FormSubmitEvent<any>) {
  // Do something with data
  console.log(event.data)
  const dataJson = JSON.stringify(state)

        const req = await fetch("http://localhost:8000/equipamento", {
            method: "POST",
            headers: {"Content-Type": "application/json"},
            body: dataJson
        })

        state.nome = undefined
        state.carga = undefined
}
function editar(index: number, equipamento: any) {
    if (equipamento.editavel) 
    {
      fetch(`${apiUrl}/equipamento/${equipamento.id}`, 
      {
        method: 'PUT',
        headers: {"Content-Type": "application/json"},
        body: JSON.stringify({ nome: equipamento.nome })
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
   
    equipamento.editavel = true;
  }
}
async function remove(id: number) 
{
  try {
    const req = await fetch(`${apiUrl}/equipamento/${id}`, {
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
        <UFormGroup label="equipamento" name="equipamento">
            <UInput v-model="state.nome" />
        </UFormGroup>

        <UFormGroup label="carga" name="carga">
            <UInput v-model="state.carga" type="carga" />
        </UFormGroup>

        <UButton type="submit">
            cadastrar
        </UButton>
        <div v-for="(equipamento, index) in equipamento" :key="index">
          <p v-if="!equipamento.editavel">{{ equipamento.nome }}</p>
          <UInput v-else v-model="equipamento.nome" />
          <UButton @click="editar(index, equipamento)">Editar</UButton>
          <UButton @click="remove(equipamento.id)">Remover</UButton>
          <hr>
        </div>
    </UForm>

    <UTable :rows="equipamento" />
</template>

