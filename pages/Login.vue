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
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
    <link href="c.css" rel="stylesheet">
    <body class="bg-gradient-to-r from-slate-950 via-neutral-400 to-slate-950">
        <div class="h-screen flex items-center justify-center">
            <div class="bg-slate-900 p-8 px-8 py-12 rounded border-md shadow-md w-full h-160 sm:w-96">
                <h2 class="text-2xl font-serif text-white mb-6 text-center">Cadastro de Usuário</h2>

                    <UForm :validate="validate" :state="state" class="space-y-4" @submit="onSubmit">
                        <div class="mb-4">
                            <UFormGroup name="usuarios" class="mb-2">
                                <label  class="block text-white font-serif mb-2">Nome de Usuário *</label>
                                <UInput v-model="state.usuarios" />
                            </UFormGroup>
                        </div>
                        <div class="mb-4">
                            <UFormGroup name="Senhas" >
                                <label  class="block text-white font-serif mb-2">Senha *</label>
                                <UInput v-model="state.senhas" type="text" />
                            </UFormGroup>
                        </div>
                        <div class="mb-14"></div>
                    
                        <UButton type="submit" class="w-full  bg-stone-600 text-white tracking-wider font-light py-2 px-4 rounded hover:bg-stone-500 flex items-center justify-center">
                            Cadastrar
                        </UButton>
                        
                    </UForm>
            </div>
        </div>
    </body>

</template>