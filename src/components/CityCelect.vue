<!-- eslint-disable no-undef -->
<script setup>
import Button from "./Button.vue";
import IconLocation from "../icons/IconLocation.vue";
import { inject, onMounted, ref } from "vue";
import Input from "./Input.vue";
const emit = defineEmits(["selectCity"]);

let isEdit = ref(false);
let city = ref("Moscow");

onMounted(() => {
  emit("selectCity", city.value);
});

function select() {
  emit("selectCity", city.value);
  isEdit.value = false;
}

function edit() {
  isEdit.value = true;
}

console.log(inject("num"))

</script>

<template>
  <div class="city-select">
    <div v-if="isEdit" class="city-input">
      <Input
        v-model="city"
        v-focus
        placeholder="Введите город"
        @keyup.enter="select"
      />
      <Button @click="select">Сохранить</Button>
    </div>
    <Button v-else @click="edit">
      <IconLocation />
      Изменить город
    </Button>
  </div>
</template>

<style scoped>
.city-input {
  display: flex;
  gap: 12px;
}
.city-select {
  width: 420px;
}
</style>
