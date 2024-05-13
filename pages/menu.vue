<template>
    <div>
        <div class="border-t-2 border-gray-800 bg-gray-900 text-white p-3 text-center">
    <p class="text-lg">SEJA-BEM VINDO AO SISTEMA DE FICHA DE TREINO</p>
  </div>
  <div class=" bg-gray-900 text-white p-0">
  <div class="w-full">
    </div>
        <button @click="toggleMenu" class="block sm:hidden fixed top-0 right-0 p-4 bg-gray-800 text-white">
        <span class="material-icons">menu</span>
      </button>
      <div class="bg-cover bg opacity-50 bg-center h-screen flex justify-center items-center border-gray-800 bg-gray-900" style="background-image: url('https://images.unsplash.com/photo-1534438327276-14e5300c3a48?q=80&w=1470&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D')">


    <div class="flex flex-col justify-center items-center h-screen">
    <div class="-m-14"></div>
  <UButton class="text-lg space-y-4 p-4 w-80 h-30 justify-center items-center tracking-wide bg-slate-950 hover:bg-slate-600 text-white">
    <a href="/Alunos">ALUNOS</a>
  </UButton>
  
  <div class="mb-6"></div>
  <UButton class="text-lg space-y-4 p-4 w-80 h-30 justify-center items-center tracking-wide bg-slate-950 hover:bg-slate-600 text-white">
    <a href="/instrutor">INSTRUTORES</a>
  </UButton>
  
  <div class="mb-6"></div>
  <UButton class="text-lg space-y-4 p-4 w-80 h-30 justify-center items-center tracking-wide bg-slate-950 hover:bg-slate-600 text-white">
    <a href="/exercicios">EXERCÍCIOS</a>
  </UButton>
  
  <div class="mb-6"></div>
  <UButton class="text-lg space-y-4 p-4 w-80 h-30 justify-center items-center tracking-wide bg-slate-950 hover:bg-slate-600 text-white">
    <a href="/aparelhos">APARELHOS</a>
  </UButton>
  
   <div class="mb-6"></div>
  <UButton class="text-lg space-y-4 p-4 w-80 h-30 justify-center items-center tracking-wide bg-slate-950 hover:bg-slate-600 text-white">
    <a href="/ficha">FICHA DE TREINO</a>
  
   </UButton>
</div>
</div>
</div>
</div> 
</template>