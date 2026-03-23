<script setup>
import { ref, provide, watch, onMounted} from "vue";
import PaneRight from "./components/PaneRight.vue";
import { API_ENDPOINT, cityProvide } from "./constants";
import PaneLeft from "./components/PaneLeft.vue";


let data = ref();
let error = ref();
let activeIndex = ref(0);
let city = ref("Moscow");

provide(cityProvide, city)

watch(city, () => {
  getCity(city.value);
})

onMounted(() => {
  getCity(city.value);
})
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
    <div class="left">
      <PaneLeft v-if="data" :day-data="data.forecast.forecastday[activeIndex]"/>
    </div>
    <div class="right">
      <PaneRight
        :data
        :error
        :active-index="activeIndex"
        @select-index="(index) => (activeIndex = index)"
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
