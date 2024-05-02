<script setup lang="ts">
import data from '../db/db.json'
import type { FormError, FormSubmitEvent } from '#ui/types'

const login = data.login


const state = reactive({
  usuarios: undefined,
  senhas: undefined
})

const validate = (state: any): FormError[] => {
  const errors = []
  if (!state.usuarios) errors.push({ path: 'usuarios', message: 'campo obrigatorio' })
  if (!state.senhas) errors.push({ path: 'senhas', message: 'campo obrigatorio' })
  return errors
}

async function onSubmit (event: FormSubmitEvent<any>) {
  // Do something with data
  console.log(event.data)
  const dataJson = JSON.stringify(state)

        const req = await fetch("http://localhost:8000/login", {
            method: "POST",
            headers: {"Content-Type": "application/json"},
            body: dataJson
        })

        state.usuarios = undefined
        state.senhas = undefined
}
</script>
<template>
    <UForm :validate="validate" :state="state" class="space-y-4" @submit="onSubmit">
        <UFormGroup label="usuarios" name="usuarios">
            <UInput v-model="state.usuarios" />
        </UFormGroup>

        <UFormGroup label="senhas" name="senhas">
            <UInput v-model="state.senhas" type="senhas" />
        </UFormGroup>

        <UButton type="submit">
            cadastrar
        </UButton>
    </UForm>

    <UTable :rows="login" />
</template>