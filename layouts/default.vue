<script setup>
import sidebar from '~/components/flowbite/sidebar.vue';
import navbar from '~/components/flowbite/navbar.vue';
</script>

<template>

  <sidebar />
  <navbar />
  
  <div class="p-4 sm:ml-64 bg-black mt-16 sm:mt-14">
    
    <slot />

  </div>
</template>
