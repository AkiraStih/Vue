<script setup>
import CityCelect from './components/CityCelect.vue';
import Error from './components/Error.vue';
import Stat from './components/Stat.vue';
import { computed, ref } from 'vue';
import IconSun from './icons/weather/IconSun.vue';
import IconCloud from './icons/weather/IconCloud.vue';
import IconRain from './icons/weather/IconRain.vue';


const API_ENDPOINT = "http://api.weatherapi.com/v1";

const errorMap = new Map([
  [1006,"Указаный город не найден"]
])

const data = ref()

const error = ref()

const errorDisplay = computed(() => {
  return errorMap.get(error?.value?.error?.code)
})


const dataModified = computed(() => {
  if (!data.value) {
    return []
  }
  return [


    {
      label: "Влажность",
      stat: `${data.value.current.humidity}%`,
    },

    {
      label: "Облачность",
      stat: `${data.value.current.cloud}%`,
    },

    {
      label: "Ветер",
      stat: `${data.value.current.wind_kph}км/ч`,
    },

  ]
})

async function getCity(city) {
  const params = new URLSearchParams({
    q: city,
    days: 3,
    lang: "ru",
    key: "26c6abc649f249a8bca222157251712"
  })
  const res = await fetch(`${API_ENDPOINT}/forecast.json?${params.toString()}`)

  if (res.status != 200) {
    error.value = await res.json()
    data.value = null
    return
  }
  error.value = null
  data.value = await res.json()

}
</script>


<template>
  <main class="main">
    <Error :error="errorDisplay" />
    <IconSun color="red"  size="54"/>
    <IconCloud />
    <IconRain />
    <Stat v-for="item in dataModified" v-bind="item" :key="item.label"></Stat>
    <CityCelect @select-city="getCity" />
  </main>
</template>

<style scoped>
.main {
  background-color: var(--color-bg-main);
  padding: 60px 50px;
  border-radius: 25px;
}
</style>
