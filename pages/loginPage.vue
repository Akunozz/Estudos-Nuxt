<script setup lang="ts">
definePageMeta({
  layout: false
});
</script>

<template>
  <div class="flex h-screen relative">
    <!-- Imagem no canto superior esquerdo -->
     <NuxtLink to="/">
    <img src="/public/img/logo-appelsoft.png" alt="Logo Appelsoft" class="absolute top-0 left-0 w-40 sm:w-64 m-4">
    </NuxtLink>
    <!-- Lado esquerdo: Logo e texto -->
    <div class="hidden sm:flex w-2/3 bg-gray-800 items-center justify-center">
      <div class="text-center">
        <img src="/img/logo.png" alt="Logo" class="w-48 mx-auto mb-4">
        <h1 class="text-white text-3xl font-bold">Checklist</h1>
      </div>
    </div>

    <!-- Lado direito: Formulário -->
    <div class="w-full sm:w-1/3 flex items-center justify-center bg-gray-700 p-6">
      <FlowbiteLoginForm />
    </div>
  </div>
</template>