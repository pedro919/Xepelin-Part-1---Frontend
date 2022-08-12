<script setup>
import axios from 'axios'

const logged = ref(false)
const bearerToken = ref(undefined)
const ratesList = ref(undefined)
const showErrorFromBackend = ref(false)
const loading = ref(false)

const getRatesList = async () => {
  const config = {
    headers: { Authorization: `Bearer ${bearerToken.value}` },
  }
  const response = await axios.get(`${import.meta.env.VITE_API_URL}/api/gspread_info`, {
    headers: {
      Authorization: `Bearer ${bearerToken.value}`,
    },
  })
  ratesList.value = response.data.rates
}

const loggedInHandler = async (username, password) => {
  loading.value = true
  const rawResponse = await axios.post(`${import.meta.env.VITE_API_URL}/api/authenticate`,
    JSON.stringify({ username, password }),
    {
      headers: {
        'Content-Type': 'application/json',
      },
    }).catch((error) => {
    if (error.response.status === 401)
      showError.value = true
  })

  if (rawResponse !== undefined) {
    if (rawResponse.status === 200) {
      bearerToken.value = rawResponse.data.msg
      await getRatesList()
      loading.value = false
      logged.value = true
    }
  }
}
const changedRateHandler = async (event, idOp, rate, email) => {
  const response = await axios.post(`${import.meta.env.VITE_API_URL}/api/update_gspread_rate`,
    JSON.stringify({ idOp, tasa: rate, email }),
    {
      headers: {
        'Authorization': `Bearer ${bearerToken.value}`,
        'Content-Type': 'application/json',
      },
    })
}
</script>

<template>
  <div>
    <section v-if="!logged">
      <LogIn
        :show-error-from-backend="showErrorFromBackend"
        @loggedIn="loggedInHandler"
      />
    </section>
    <section>
      <div v-if="loading" class="mt-14 text-4xl text-center text-gray-900">
        Cargando...
      </div>
    </section>
    <section v-if="logged">
      <TheNavbar
        @loggedOut="logged = false"
      />
      <GSpreadSheet
        :rates-list="ratesList"
        @changedRate="changedRateHandler"
      />
    </section>
  </div>
</template>

