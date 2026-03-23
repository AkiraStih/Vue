<!-- eslint-disable no-undef -->
<script setup>
import Button from "./Button.vue";
import IconLocation from "../icons/IconLocation.vue";
import { inject, onMounted, ref } from "vue";
import Input from "./Input.vue";
import { cityProvide } from "../constants";
const emit = defineEmits(["selectCity"]);

let isEdit = ref(false);
let city = inject(cityProvide);
let inputValue = ref(city.value);


onMounted(() => {
  emit("selectCity", city.value);
});

function select() {
  isEdit.value = false;
  city.value = inputValue.value;
}

function edit() {
  isEdit.value = true;
}


</script>

<template>
  <div class="city-select">
    <div v-if="isEdit" class="city-input">
      <Input
        v-model="inputValue"
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
