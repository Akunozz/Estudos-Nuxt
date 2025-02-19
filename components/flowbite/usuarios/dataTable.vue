<template>

  <div class="container mx-auto p-6 ">
    <div class="flex justify-between">
      <h1 class="text-xl text-white font-semibold mb-4">Lista de usuários cadastrados</h1>
      <cadastro @usuarioCadastrado="refreshMotoristas" />
    </div>

    <div class="relative overflow-x-auto shadow-md sm:rounded-lg">
      <table class="w-full text-sm text-center text-gray-500 dark:text-gray-400">
        <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
          <tr>
            <th scope="col" class="py-3 text-gray-700 bg-gray-50 dark:bg-gray-700 dark:text-gray-100">Foto</th>

            <th scope="col" class="py-3">
              <input v-model="filtroNome" type="text" placeholder="Nome"
                class="border rounded-md text-gray-700 bg-gray-50 dark:bg-gray-700 dark:text-gray-100" />
            </th>

            <th scope="col" class="py-3">
              <div class="flex flex-col items-center">
                <span class="text-gray-700 bg-gray-50 dark:bg-gray-700 dark:text-gray-100 mb-1">Data nascimento</span>
                <input v-model="filtroAniversario" type="date"
                  class="border rounded-md text-gray-700 bg-gray-50 dark:bg-gray-700 dark:text-gray-100" />
              </div>
            </th>

            <th scope="col" class="py-3">
              <div class="flex flex-col items-center">
                <span class="text-gray-700 bg-gray-50 dark:bg-gray-700 dark:text-gray-100 mb-1">Validade CNH</span>
                <input v-model="filtroValidadeCNH" type="date"
                  class="border rounded-md text-gray-700 bg-gray-50 dark:bg-gray-700 dark:text-gray-100" />
              </div>
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="motorista in motoristasPaginados" :key="motorista.id"
            class="bg-white border-b dark:bg-gray-800 dark:border-gray-700">
            <td class="px-2 py-3">
              <img :src="motorista.avatar" alt="Avatar" class="ml-16 w-12 h-12 rounded-full" />
            </td>
            <td class="px-2 py-3 font-medium text-gray-900 dark:text-white">{{ motorista.nome }}</td>
            <td class="px-2 py-3">{{ formatarData(motorista.aniversario) }}</td>
            <td class="px-2 py-3">{{ formatarData(motorista.validadeCNH) }}</td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- Controles de Paginação -->
    <div class="flex justify-center items-center mt-4 gap-4">
      <button @click="paginaAtual--" :disabled="paginaAtual === 1"
        class="px-4 py-2 bg-gray-700 text-white rounded-md">Anterior</button>
      <span class="text-white">Página</span>
      <input v-model.number="paginaAtual" type="number" min="1" :max="totalPaginas"
        class="w-16 border rounded-md" />
      <span class="text-white">de {{ totalPaginas }}</span>
      <button @click="paginaAtual++" :disabled="paginaAtual >= totalPaginas"
        class="px-4 py-2 bg-gray-700 text-white rounded-md">Próxima</button>
    </div>
  </div>
</template>

<script setup>
import { computed, ref } from "vue";
import { useAsyncData } from "#app";
import cadastro from "~/components/flowbite/usuarios/cadastro.vue";

const filtroNome = ref("");
const filtroAniversario = ref("");
const filtroValidadeCNH = ref("");
const paginaAtual = ref(1);
const itensPorPagina = 10;

// Buscando os motoristas com useAsyncData
const { data: motoristas, refresh } = useAsyncData(
  "motoristas",
  () => fetch("https://67a9ea7365ab088ea7e4f65e.mockapi.io/api/estudo/motorista")
          .then((res) => res.json()),
  { server: false, lazy: true }
);

// Função para atualizar a lista após um cadastro
const refreshMotoristas = () => {
  refresh();
};

const formatarData = (data) => {
  if (!data) return "";
  return new Date(data).toLocaleDateString("pt-BR");
};

// Computed para filtrar os motoristas
const motoristasFiltrados = computed(() => {
  return motoristas.value?.filter((motorista) => {
    const nomeMatch = motorista.nome.toLowerCase().includes(filtroNome.value.toLowerCase());
    const aniversarioMatch = filtroAniversario.value ? motorista.aniversario.startsWith(filtroAniversario.value) : true;
    const validadeCNHMatch = filtroValidadeCNH.value ? motorista.validadeCNH.startsWith(filtroValidadeCNH.value) : true;
    return nomeMatch && aniversarioMatch && validadeCNHMatch;
  }) || [];
});

// Paginação
const totalPaginas = computed(() => Math.ceil(motoristasFiltrados.value.length / itensPorPagina));
const motoristasPaginados = computed(() => {
  const inicio = (paginaAtual.value - 1) * itensPorPagina;
  return motoristasFiltrados.value.slice(inicio, inicio + itensPorPagina);
});
</script>
