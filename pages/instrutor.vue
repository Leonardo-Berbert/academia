<script setup lang="ts">
import data from '../db/db.json'
import type { FormError, FormSubmitEvent } from '#ui/types'

const instrutor = data.instrutor


const state = reactive({
  nome: undefined,
  cpf: undefined
})

const validate = (state: any): FormError[] => {
  const errors = []
  if (!state.cpf) errors.push({ path: 'cpf', message: 'campo obrigatorio' })
  if (!state.nome) errors.push({ path: 'nome', message: 'campo obrigatorio' })
  return errors
}

async function onSubmit (event: FormSubmitEvent<any>) {
  // Do something with data
  console.log(event.data)
  const dataJson = JSON.stringify(state)

        const req = await fetch("http://localhost:8000/instrutor", {
            method: "POST",
            headers: {"Content-Type": "application/json"},
            body: dataJson
        })

        state.nome = undefined
        state.cpf = undefined
}
</script>

<template>
    <UForm :validate="validate" :state="state" class="space-y-4" @submit="onSubmit">
        <UFormGroup label="instrutor" name="instrutor">
            <UInput v-model="state.nome" />
        </UFormGroup>

        <UFormGroup label="cpf" name="cpf">
            <UInput v-model="state.cpf" type="carga" />
        </UFormGroup>

        <UButton type="submit">
            cadastrar
        </UButton>
    </UForm>

    <UTable :rows="instrutor" />
</template>
