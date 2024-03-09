<template>
    <UFormGroup v-slot="{ error }" label="nome" :error="!name && 'escreva um nome'" help="nome aceito!">
      <UInput v-model="name" type="email" placeholder="coloque seu nome" :trailing-icon="error ? 'i-heroicons-exclamation-triangle-20-solid' : undefined" />
    </UFormGroup>
</template>
  
<script setup lang="ts">
    const name = ref('')
</script>
  
  