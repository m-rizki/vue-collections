<script setup>
import { ref } from 'vue'

const text = ref('')
</script>

<template>
  <!--  v-model automatically syncs the <input>'s value with the bound state  -->
  <input v-model="text" placeholder="Type here">
  <p>{{ text }}</p>
</template>