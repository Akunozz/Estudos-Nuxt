<script setup>
import { ref, onMounted } from 'vue';

const emailInput = ref(null);

onMounted(() => {
    emailInput.value?.focus();
});
</script>

<template>
    <form class="max-w-sm md:max-w-md lg:max-w-lg mx-auto w-full px-4">

        <h1 class="text-2xl font-medium text-gray-900 dark:text-white mb-8"> Seja bem vindo!</h1>

        <div class="mb-5">
            <label for="email" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Email</label>
            <input ref="emailInput" type="email" id="email"
                class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                placeholder="Email" required />
        </div>
        
        <div class="mb-5">
            <label for="password" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Senha</label>
            <input type="password" id="password"
                class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                placeholder="********" required />
        </div>

        <div class="flex items-center justify-between gap-2 sm:gap-0 mb-5">
            <div class="flex items-center">
                <input id="remember" type="checkbox"
                    class="w-4 h-4 border border-gray-300 rounded-sm bg-gray-50 focus:ring-3 focus:ring-blue-300 dark:bg-gray-700 dark:border-gray-600 dark:focus:ring-blue-600 dark:ring-offset-gray-800 dark:focus:ring-offset-gray-800" />
                <label for="remember" class="ms-2 text-sm font-medium text-gray-900 dark:text-gray-300">
                    Lembrar senha
                </label>
            </div>
            <a href="#" class="text-sm text-blue-400 hover:underline">Esqueceu a senha?</a>
        </div>

        <button type="submit"
            class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm w-full px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">
            Login
        </button>

    </form>
    
</template>