<template>
    <div>
      <h1>Selecionar Dados</h1>
      <select v-model="selectedType" @change="onChange">
        <option value="aluno">Alunos</option>
        <option value="instrutor">Instrutores</option>
        <option value="equipamento">Equipamentos</option>
        <option value="exercicio">Exercícios</option>
      </select>
  
      <div v-if="selectedData">
        <h2 v-if="selectedType === 'aluno'">Lista de Alunos</h2>
        <h2 v-if="selectedType === 'instrutor'">Lista de Instrutores</h2>
        <h2 v-if="selectedType === 'equipamento'">Lista de Equipamentos</h2>
        <h2 v-if="selectedType === 'exercicio'">Lista de Exercícios</h2>
  
        <table v-if="selectedData.length">
          <thead>
            <tr>
              <th>ID</th>
              <th>Nome</th>
              <th v-if="selectedType === 'aluno'">CPF</th>
              <th v-if="selectedType === 'instrutor'">CPF</th>
              <th v-if="selectedType === 'equipamento'">Carga</th>
              <th v-if="selectedType === 'exercicio'">Aparelho</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in selectedData" :key="item.id">
              <td>{{ item.id }}</td>
              <td>{{ item.nome }}</td>
              <td v-if="selectedType === 'aluno'">{{ item.cpf }}</td>
              <td v-if="selectedType === 'instrutor'">{{ item.cpf }}</td>
              <td v-if="selectedType === 'equipamento'">{{ item.carga }}</td>
              <td v-if="selectedType === 'exercicio'">{{ item.aparelho }}</td>
            </tr>
          </tbody>
        </table>
        <p v-else>Nenhum dado disponível.</p>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue'
  import data from '../db/db.json'
  
  const alunos = data.aluno
  const instrutores = data.instrutor
  const equipamentos = data.equipamento
  const exercicios = data.exercicio
  
  const selectedType = ref('aluno')
  const selectedData = ref(alunos)
  
  function onChange() {
    switch (selectedType.value) {
      case 'aluno':
        selectedData.value = alunos
        break
      case 'instrutor':
        selectedData.value = instrutores
        break
      case 'equipamento':
        selectedData.value = equipamentos
        break
      case 'exercicio':
        selectedData.value = exercicios
        break
      default:
        selectedData.value = []
        break
    }
  }
  </script>