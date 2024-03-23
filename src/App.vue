<script setup lang="ts">
import TrelloBoard from './components/TrelloBoard.vue'
</script>

<template>
  <div class="p-10 h-[100vh] bg-teal-600 overflow-auto">
    <h1 class="text-4xl text-white flex items-center mb-10">
      <img
        width="200"
        class="mr-3"
        src="https://vueschool.io/img/logo/vueschool_logo_multicolor_negative.svg"
        alt="Vue School"
      />
      Trello Board
    </h1>
    <TrelloBoard />
  </div>
</template>

<style scoped>
</style>
