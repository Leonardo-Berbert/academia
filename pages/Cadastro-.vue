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
  if (!state.usuarios) errors.push({ path: 'usuarios', message: 'Campo obrigatório' })
  if (!state.senhas) errors.push({ path: 'senhas', message: 'Campo obrigatório' })
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
<body class="bg-green-100">
    <div class="min-h-screen flex items-center justify-center">
        <div class="bg-neutral-800 p-8 rounded border-md shadow-md w-full h-full sm:w-96">
            <h2 class="text-2xl font-serif text-white mb-6 text-center">Cadastro de Usuário</h2>
            <UForm :validate="validate" :state="state" class="space-y-4" @submit="onSubmit">
            <div class="mb-4">
                <UFormGroup label="Usuario *" class="block text-black font-serif mb-2">
                    <UInput v-model="state.usuarios"  type="text" id="username" name="username" class="w-full h-9 px-3 py-2 border text- bg-none border-gray-100 bg-emerald-400 rounded focus:outline-none focus:border-slate-800" required/>
                </UFormGroup>
            </div>

            <div class="mb-4">
                <UFormGroup label="Senha *" class="block text-black font-serif mb-2">
                    <UInput v-model="state.senha"  type="text" id="username" name="username" class="w-full h-9 px-3 py-2 border text- bg-none border-gray-100 bg-emerald-400 rounded focus:outline-none focus:border-slate-800" required/>
                </UFormGroup>
            </div>
            <div class="mb-8"></div>
                <UButton type="submit" class="w-full  bg-stone-600 text-white tracking-wider font-light py-2 px-4 rounded hover:bg-stone-500">Cadastrar</UButton>
          
         </UForm>
        </div>
    </div>
</body>



</template>