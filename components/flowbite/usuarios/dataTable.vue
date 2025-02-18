<template>
    <div class="container mx-auto p-6">
      <h1 class="text-xl text-white font-semibold mb-4">Lista de usuários cadastrados</h1>
  
      <!-- Campos de filtro -->
      <div class="mb-4 flex flex-wrap gap-4">
        <input v-model="filtroNome" type="text" placeholder="Filtrar por nome" class="p-2 border rounded-md" />
        <input v-model="filtroAniversario" type="date" placeholder="Filtrar por aniversário" class="p-2 border rounded-md" />
        <input v-model="filtroValidadeCNH" type="date" placeholder="Filtrar por validade CNH" class="p-2 border rounded-md" />
      </div>
  
      <div class="relative overflow-x-auto shadow-md sm:rounded-lg">
        <table class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
          <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
            <tr>
              <th scope="col" class="px-6 py-3">Avatar</th>
              <th scope="col" class="px-6 py-3">Nome</th>
              <th scope="col" class="px-6 py-3">Aniversário</th>
              <th scope="col" class="px-6 py-3">Validade CNH</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="motorista in motoristasPaginados" :key="motorista.id" class="bg-white border-b dark:bg-gray-800 dark:border-gray-700">
              <td class="px-6 py-4">
                <img :src="motorista.avatar" alt="Avatar" class="w-10 h-10 rounded-full">
              </td>
              <td class="px-6 py-4 font-medium text-gray-900 dark:text-white">{{ motorista.nome }}</td>
              <td class="px-6 py-4">{{ formatarData(motorista.aniversario) }}</td>
              <td class="px-6 py-4">{{ formatarData(motorista.validadeCNH) }}</td>
            </tr>
          </tbody>
        </table>
      </div>
  
      <!-- Controles de Paginação -->
      <div class="flex justify-center items-center mt-4 gap-4">
        <button @click="paginaAtual--" :disabled="paginaAtual === 1" class="px-4 py-2 bg-gray-700 text-white rounded-md">Anterior</button>
        <span>Página</span>
        <input v-model.number="paginaAtual" type="number" min="1" :max="totalPaginas" class="w-16 text-center border rounded-md" />
        <span>de {{ totalPaginas }}</span>
        <button @click="paginaAtual++" :disabled="paginaAtual >= totalPaginas" class="px-4 py-2 bg-gray-700 text-white rounded-md">Próxima</button>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, computed, onMounted, watch } from "vue";
  import { useFetch } from "#app";
  
  const motoristas = ref([]);
  const filtroNome = ref("");
  const filtroAniversario = ref("");
  const filtroValidadeCNH = ref("");
  const paginaAtual = ref(1);
  const itensPorPagina = 10;
  
  const fetchMotoristas = async () => {
    try {
      const { data } = await useFetch("https://67a9ea7365ab088ea7e4f65e.mockapi.io/api/estudo/motorista");
      motoristas.value = data.value || [];
    } catch (error) {
      console.error("Erro ao buscar motoristas:", error);
    }
  };
  
  const formatarData = (data) => {
    if (!data) return "";
    return new Date(data).toLocaleDateString("pt-BR");
  };
  
  // Computed para filtrar os motoristas
  const motoristasFiltrados = computed(() => {
    return motoristas.value.filter(motorista => {
      const nomeMatch = motorista.nome.toLowerCase().includes(filtroNome.value.toLowerCase());
      const aniversarioMatch = filtroAniversario.value ? motorista.aniversario.startsWith(filtroAniversario.value) : true;
      const validadeCNHMatch = filtroValidadeCNH.value ? motorista.validadeCNH.startsWith(filtroValidadeCNH.value) : true;
      return nomeMatch && aniversarioMatch && validadeCNHMatch;
    });
  });
  
  // Paginação
  const totalPaginas = computed(() => Math.ceil(motoristasFiltrados.value.length / itensPorPagina));
  const motoristasPaginados = computed(() => {
    const inicio = (paginaAtual.value - 1) * itensPorPagina;
    return motoristasFiltrados.value.slice(inicio, inicio + itensPorPagina);
  });
  
  // Ajusta a página se o número inserido estiver fora do intervalo válido
  watch(paginaAtual, (novaPagina) => {
    if (novaPagina < 1) paginaAtual.value = 1;
    if (novaPagina > totalPaginas.value) paginaAtual.value = totalPaginas.value;
  });
  
  onMounted(fetchMotoristas);
  </script>
  