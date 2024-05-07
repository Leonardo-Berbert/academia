<script setup>
import { ref } from 'vue'
import data from '../db/db.json'

const aluno = data.aluno
const equipamento = data.equipamento
const exercicio = data.exercicio
const instrutor = data.instrutor

</script>

<template>
 
  <div>
    <label for="aluno">Escolha um aluno:</label>
    <select id="aluno" v-model="aluno.nome">
      <option v-for="aluno in aluno" :key="aluno.id" :value="aluno">{{ aluno.nome }}</option>
    </select>
    <label for="equipamento">Escolha um equipamento:</label>
    <select id="equipamento" v-model="equipamento.nome">
      <option v-for="equipamento in equipamento" :key="equipamento.id" :value="equipamento">{{ equipamento.nome }}</option>
    </select>
    <label for="exercicio">Escolha um exercicio:</label>
    <select id="exercicio" v-model="exercicio.nome">
      <option v-for="exercicio in exercicio" :key="exercicio.id" :value="exercicio">{{ exercicio.nome }}</option>
    </select>
    <label for="instrutor">Escolha um instrutor:</label>
    <select id="instrutor" v-model="instrutor.nome">
      <option v-for="instrutor in instrutor" :key="instrutor.id" :value="instrutor">{{ instrutor.nome }}</option>
    </select>
  </div>
</template>