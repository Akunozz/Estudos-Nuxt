<script setup>
import { ref, computed } from "vue";

const isCollapsed = ref(false); // Estado do sidebar

// Computa a classe da margem do conteúdo principal
const mainContentClasses = computed(() => {
  return isCollapsed.value
    ? "p-4 ml-20 bg-black mt-16 sm:mt-14 transition-all"
    : "p-4 ml-64 bg-black mt-16 sm:mt-14 transition-all";
});

const toggleSidebar = () => {
  isCollapsed.value = !isCollapsed.value;
};
</script>

<template>
  <FlowbiteSidebar :collapsed="isCollapsed" @toggle="toggleSidebar" />
  <FlowbiteNavbar />

  <div :class="mainContentClasses">
    <slot />
  </div>
</template>
