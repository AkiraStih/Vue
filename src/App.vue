<script setup>
import { ref, provide} from "vue";
import PaneRight from "./components/PaneRight.vue";

const API_ENDPOINT = "http://api.weatherapi.com/v1";

let data = ref();
let error = ref();
let activeIndex = ref(0);
provide("num", 1)
async function getCity(city) {
  const params = new URLSearchParams({
    q: city,
    days: 3,
    lang: "ru",
    key: "26c6abc649f249a8bca222157251712",
  });
  const res = await fetch(`${API_ENDPOINT}/forecast.json?${params.toString()}`);

  if (res.status != 200) {
    error.value = await res.json();
    data.value = null;
    return;
  }
  error.value = null;
  data.value = await res.json();
}
</script>

<template>
  <main class="main">
    <div class="left"></div>
    <div class="right">
      <PaneRight
        :data
        :error
        :active-index="activeIndex"
        @select-index="(i) => (activeIndex = i)"
        @select-city="getCity"
      />
    </div>
  </main>
</template>

<style scoped>
.main {
  display: flex;
  justify-content: center;
  align-items: center;
}
.left {
  width: 500px;
  height: 660px;
  border-radius: 30px;
  background-image: url("/public/bg.png");
  background-repeat: no-repeat;
  background-size: cover;
}
.right {
  background-color: var(--color-bg-main);
  padding: 60px 50px;
  border-radius: 25px;
}

</style>
