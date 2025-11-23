<script setup>
import { ref } from 'vue'

const data = ref("")
</script>

<template>
  <input v-model="data" placeholder="отредактируй меня" >
  <div class="data">{{ data }}</div>
</template>

<style scoped>
  .data{
    font-size: 20px;
    color: beige;
  }
</style>