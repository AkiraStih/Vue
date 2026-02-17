<script setup>

const city = defineModel()


</script>

<template>
  <input v-model="city" class="input" />
</template>

<style scoped>
.input {
  color: var(--color-primary);
  border-radius: 10px;
  background: var(--color-bg);
  box-shadow: 1px 2px 4px 0px var(--color-bg-main);
  border: none;
  font-size: 18px;
  padding: 15px 20px;
  width: 260px;
  height: 53px;

}

.input::placeholder {
  color: #3F4958;
}
</style>