<script setup lang="ts">
import data from '../db/db.json'
import type { FormError, FormSubmitEvent } from '#ui/types'

const exercicio = data.exercicio


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
    </UForm>

    <UTable :rows="exercicio" />
</template>

