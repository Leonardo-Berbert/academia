<template>
  <header>
   <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.2/css/all.min.css" integrity="sha512-SnH5WK+bZxgPHs44uWIX+LLJAJ9/2PkPKZ5QiAj6Ta86w+fsb2TkcmfRyVX3pBnMFcV7oQPJkl9QevSCWr3W6A==" crossorigin="anonymous" referrerpolicy="no-referrer" />
  <div class="border-t-2 color-box  border-gray-800 bg-gray-900 text-white p-2 text-left flex justify-between items-center "> 
    <a href="/menu" class="text-white hover:text-slate-400">HOME</a>
    <p class="text-lg ml-30 mr-20 color-box">Ficha de Treino Especializada</p>
    <button @click="imprimir" class="p-2 bg-blue-500 text-white rounded mr-3 hover:bg-blue-600">
      <i class="fa-solid fa-print fa-lg" style="color: #dde3ee;"></i>
    </button>
  </div>
</header>
  <div class="flex flex-wrap gap-4 bg-slate-100">
    <div class="w-full flex flex-col bg-slate-100">
      <div class="flex flex-wrap gap-4 justify-between items-start bg-slate-100">
        <div class="w-full sm:w-5/12 flex flex-col ml-16">
          <label for="aluno" class="mb-1 m-4 ml-1 font-medium">Aluno:</label>
          <select id="aluno" v-model="alunoSelecionado" class="p-2 border border-black rounded">
            <option v-for="aluno in alunos" :key="aluno.id" :value="aluno">{{ aluno.nome }}</option>
          </select>
        </div>
        <div class="w-full sm:w-5/12 flex flex-col mr-20">
          <label for="instrutor" class="mb-1 m-4 ml-1 font-medium">Instrutor:</label>
          <select id="instrutor" v-model="instrutorSelecionado" class="p-2 border border-black rounded">
            <option v-for="instrutor in instrutores" :key="instrutor.id" :value="instrutor">{{ instrutor.nome }}</option>
          </select>
        </div>
        <div class="w-full flex flex-col mb-4 mr-16 ml-8">
          <label for="objetivo" class="mb-1 m-2 text-center font-medium">Objetivo:</label>
          <select v-model="objetivoSelecionado" id="objetivo" class="p-2 ml-8 mr-4 border border-black rounded text-center">
            <option v-for="exercicio in exercicios" :key="exercicio.id" :value="exercicio.objetivo">{{ exercicio.objetivo }}</option>
          </select>
        </div>
        <div v-for="(item, index) in exerciciosSelecionados" :key="index" class="w-full flex flex-wrap gap-4 justify-between items-start ml-8 mr-16">
          <div class="flex flex-col mb-4">
            <label :for="'exercicio_' + index" class="mb-1 ml-7 font-medium">Exercício:</label>
            <select v-model="item.exercicio" :id="'exercicio_' + index" class="p-2 ml-7 w-64 border border-black rounded">
              <option v-for="exercicio in exercicios" :key="exercicio.id" :value="exercicio">{{ exercicio.nome }}</option>
            </select>
          </div>
          <div class="flex flex-col mb-4">
            <label :for="'serie_' + index" class="mb-1 font-medium">Nº de séries:</label>
            <select v-model="item.series" :id="'serie_' + index" class="p-2 border border-black rounded">
              <option v-for="serie in series" :key="serie">{{ serie }}</option>
            </select>
          </div>
          <div class="flex flex-col mb-4">
            <label :for="'repeticao_' + index" class="mb-1 font-medium">Nº de repetições:</label>
            <select v-model="item.repeticoes" :id="'repeticao_' + index" class="p-2 border w-36 border-black rounded">
              <option v-for="repeticao in repeticoes" :key="repeticao">{{ repeticao }}</option>
            </select>
          </div>
          <div class="flex flex-col mb-4">
            <label :for="'aparelho_' + index" class="mb-1 font-medium">Aparelho:</label>
            <select v-model="item.aparelho" :id="'aparelho_' + index" class="p-2 border w-64 border-black rounded">
              <option v-for="aparelho in aparelhos" :key="aparelho.id" :value="aparelho">{{ aparelho.nome }}</option>
            </select>
          </div>
          <div class="flex flex-col mb-4">
            <label :for="'carga_' + index" class="mb-1 font-medium">Carga:</label>
            <select v-model="item.carga" :id="'carga_' + index" class="p-2 w-36 border border-black rounded">
              <option v-for="carga in cargas" :key="carga">{{ carga.carga }}</option>
            </select>
          </div>
          <button @click="adicionarExercicio" class="mt-6 p-3 w-24 m-3 -h-10 mb-8 ml-8 bg-green-500 text-white rounded hover:bg-green-600 print-hidden">
            <i class="fa-solid fa-plus fa-xl" style="color: #d7dce5;"></i>
          </button>
          <button @click="removerExercicio(index)" class="mt-6 p-3 w-24 m-3 -h-10 bg-red-500 text-white rounded hover:bg-red-600 print-hidden">
            <i class="fa-solid fa-trash-can fa-lg" style="color: #d8dee9;"></i>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import data from '../db/db.json'

const alunos = data.aluno
const aparelhos = data.aparelho
const exercicios = data.exercicio
const instrutores = data.instrutor
const cargas = data.carga
const series = [0,1, 2, 3, 4, 5]
const repeticoes = [0,5, 10, 15, 20,25,30,35,40,45,50]

const alunoSelecionado = ref(null)
const instrutorSelecionado = ref(null)
const objetivoSelecionado = ref(null)

const exerciciosSelecionados = ref([
  { exercicio: null, series: null, repeticoes: null, aparelho: null, carga: null },
  { exercicio: null, series: null, repeticoes: null, aparelho: null, carga: null },
  { exercicio: null, series: null, repeticoes: null, aparelho: null, carga: null }
])

const adicionarExercicio = () => {
  if (exerciciosSelecionados.value.length < 5) {
    exerciciosSelecionados.value.push({ exercicio: null, series: null, repeticoes: null, aparelho: null, carga: null })
  }
}

const removerExercicio = (index) => {
  exerciciosSelecionados.value.splice(index, 1)
}

const imprimir = () => {
  window.print();
}
</script>

<style>
@media print {
  .print-hidden {
    display: none !important;
  }
  .color-box {
    -webkit-print-color-adjust: exact;
    print-color-adjust: exact;
  }
}
</style>
