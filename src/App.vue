<script setup>
import Button from "./components/Button.vue";
let  b = "Контент";
let color = "red";
</script>

<template>
  <header :class="color">Лого</header>
  <main>{{b}}
    <Button>Сохранить</Button>
  </main>

  <footer>Подвал</footer>
</template>

<style scoped>
.blue{
  color: aqua;
}

.red{
  color: brown;
}
</style>
