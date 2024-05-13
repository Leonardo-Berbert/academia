<script setup>
import { ref } from 'vue'
import data from '../db/db.json' 

const alunos = data.aluno
const aparelhos = data.aparelho
const exercicios = data.exercicio
const instrutores = data.instrutor
const carga = data.carga

const alunoSelecionado = ref(null)
const aparelhoselecionado = ref(null)
const exercicioSelecionado = ref(null)
const instrutorSelecionado = ref(null)
const cargaSelecionado = ref(null)
const objetivoSelecionado = ref(null)

const objetivosUnicos = computed(() => {
  const objetivos = new Set()
  exercicios.forEach(exercicio => {
    objetivos.add(exercicio.objetivo)
  })
  return Array.from(objetivos)
})

</script>

<template>
  <div class="flex flex-wrap gap-4">
    <div class="w-full sm:w-1/2 flex flex-col">
      <label for="aluno" class="mb-1">Escolha um aluno:</label>
      <select id="aluno" v-model="alunoSelecionado" class="p-2 border border-gray-300 rounded">
        <option v-for="aluno in alunos" :key="aluno.id" :value="aluno">{{ aluno.nome }}</option>
      </select>
      <label for="aparelho" class="mt-4 mb-1">Escolha um aparelho:</label>
      <div class="flex items-center">
        <select id="aparelho" v-model="aparelhoSelecionado" class="p-2 border border-gray-300 rounded flex-1">
          <option v-for="aparelho in aparelhos" :key="aparelho.id" :value="aparelho">{{ aparelho.nome }}</option>
        </select>
        <label for="repeticoes_exercicio" class="ml-2">Repetições:</label>
        <select id="repeticoes_aparelho" v-model="repeticoesAparelho" class="p-2 border border-gray-300 rounded w-20 ml-2">
          <option>1</option>
          <option>2</option>
          <option>3</option>
          <!-- Adicione mais opções conforme necessário -->
        </select>
      </div>
    </div>
    <div class="w-full sm:w-1/2 flex flex-col">
      <label for="exercicio" class="mb-1">Escolha um exercício:</label>
      <div class="flex items-center">
        <select id="exercicio" v-model="exercicioSelecionado" class="p-2 border border-gray-300 rounded flex-1">
          <option v-for="exercicio in exercicios" :key="exercicio.id" :value="exercicio">{{ exercicio.nome }}</option>
        </select>
        <label for="repeticoes_aparelho" class="ml-2">Repetições:</label>
        <select id="repeticoes_exercicio" v-model="repeticoesExercicio" class="p-2 border border-gray-300 rounded w-20 ml-2">
          <option>1</option>
          <option>2</option>
          <option>3</option>
          <!-- Adicione mais opções conforme necessário -->
        </select>
      </div>
      <label for="objetivo" class="mt-4 mb-1">Escolha um objetivo:</label>
      <select id="objetivo" v-model="objetivoSelecionado" class="p-2 border border-gray-300 rounded">
        <option v-for="objetivo in objetivosUnicos" :key="objetivo" :value="objetivo">{{ objetivo }}</option>
      </select>
    </div>
    <div class="w-full sm:w-1/2 flex flex-col">
      <label for="instrutor" class="mb-1">Escolha um instrutor:</label>
      <select id="instrutor" v-model="instrutorSelecionado" class="p-2 border border-gray-300 rounded">
        <option v-for="instrutor in instrutores" :key="instrutor.id" :value="instrutor">{{ instrutor.nome }}</option>
      </select>
      <label for="carga" class="mt-4 mb-1">Escolha uma carga:</label>
      <select id="carga" v-model="cargaSelecionada" class="p-2 border border-gray-300 rounded">
        <option v-for="cargaItem in carga" :key="cargaItem.id" :value="cargaItem">{{ cargaItem.carga }}</option>
      </select>
    </div>
  </div>
</template>