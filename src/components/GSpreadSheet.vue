<script setup>
import { onMounted } from 'vue'

defineProps({
  ratesList: { type: Array, required: true },
})

const emit = defineEmits(['changedRate'])
</script>

<template>
  <div class="px-4 sm:px-6 lg:px-8 mt-14">
    <div class="sm:flex sm:items-center">
      <div class="sm:flex-auto">
        <h1 class="text-xl font-semibold text-gray-900">
          Tasas
        </h1>
        <p class="mt-2 text-sm text-gray-700">
          Al modificar una tasa se realizará un post que gatillará que se envíe un email a la dirección correspondiente
        </p>
      </div>
    </div>
    <div class="mt-8 flex flex-col">
      <div class="-my-2 -mx-4 overflow-x-auto sm:-mx-6 lg:-mx-8">
        <div class="inline-block min-w-full py-2 align-middle md:px-6 lg:px-8">
          <div class="overflow-hidden shadow ring-1 ring-black ring-opacity-5 md:rounded-lg">
            <table class="min-w-full divide-y divide-gray400">
              <thead class="bg-gray-50">
                <tr>
                  <th scope="col" class="py-3.5 pl-4 pr-3 text-center text-sm font-semibold text-gray-900 sm:pl-6 lg:w-1/3">
                    id Op
                  </th>
                  <th scope="col" class="px-3 py-3.5 text-center text-sm font-semibold text-gray-900 lg:w-1/3">
                    Tasa
                  </th>
                  <th scope="col" class="px-3 py-3.5 text-center text-sm font-semibold text-gray-900 lg:w-1/3">
                    Email
                  </th>
                </tr>
              </thead>
              <tbody class="divide-y divide-gray-300 bg-white">
                <tr v-for="row in ratesList" :key="row.idOp" class="h-full">
                  <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6 text-center">
                    {{ row.idOp }}
                  </td>
                  <td class="whitespace-nowrap px-3 text-sm text-gray-500 text-center h-full">
                    <input v-model="row.rate" step=".01" type="number" class="text-gray-900 text-center border-gray-900 border rounded-md w-full h-10" placeholder="Tasa" @change="$emit('changedRate', $event, row.idOp, row.rate, row.email)">
                  </td>
                  <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500 text-center">
                    {{ row.email }}
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
}
</style>
