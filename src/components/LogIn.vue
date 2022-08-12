<script setup>
import axios, { AxiosError } from 'axios'

defineProps({
  showErrorFromBackend: { type: Boolean, required: true },
})

const emit = defineEmits(['loggedIn'])
const showErrorFromFrontend = ref(false)
const formData = reactive({ username: '', password: '' })

const loginButtonHandler = async () => {
  if (formData.username === '' || formData.password === '')
    showError.value = true
  else
    emit('loggedIn', formData.username, formData.password)
}
</script>

<template>
  <div class="min-h-full flex flex-col justify-center py-12 sm:px-6 lg:px-8">
    <div class="sm:mx-auto sm:w-full sm:max-w-md">
      <img class="mx-auto h-12 w-auto" src="https://tailwindui.com/img/logos/workflow-mark-orange-500.svg" alt="Workflow">
      <h2 class="mt-6 text-center text-3xl font-extrabold text-gray-900">
        Ingresa a tu cuenta
      </h2>
    </div>

    <div class="mt-8 sm:mx-auto sm:w-full sm:max-w-md">
      <div class="bg-white py-8 px-4 shadow sm:rounded-lg sm:px-10">
        <div class="space-y-6">
          <div>
            <label for="email" class="block text-sm font-medium text-gray-700"> Correo electrónico </label>
            <div class="mt-1">
              <input id="email" v-model="formData.username" name="email" type="email" autocomplete="email" required="true" class="appearance-none block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm placeholder-gray-400 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
            </div>
          </div>

          <div>
            <label for="password" class="block text-sm font-medium text-gray-700"> Contraseña </label>
            <div class="mt-1">
              <input id="password" v-model="formData.password" name="password" type="password" autocomplete="current-password" required="true" class="appearance-none block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm placeholder-gray-400 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
            </div>
            <p v-if="showErrorFromFrontend || showErrorFromBackend" class="text-red mt-2">
              *Usuario o contraseña incorrecto
            </p>
          </div>

          <div>
            <button class="w-full flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-orange-500 hover:bg-orange-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500" @click="loginButtonHandler">
              Sign in
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
