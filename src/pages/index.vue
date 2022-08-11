<script setup>
const logged = ref(false)
</script>

<template>
  <div v-if="!logged">
    <LogIn
      @loggedIn="logged = true"
    />
  </div>
  <div v-if="logged">
    <TheNavbar />
    <GSpreadSheet class="mt-14" />
  </div>
</template>

