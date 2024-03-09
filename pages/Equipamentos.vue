<script setup lang="ts">
import data from '../db/db.json'
import type { FormError, FormSubmitEvent } from '#ui/types'

const equipamento = data.equipamento


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
    </UForm>

    <UTable :rows="equipamento" />
</template>

