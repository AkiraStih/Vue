<script setup>
import CityCelect from './components/CityCelect.vue';
import Error from './components/Error.vue';
import Stat from './components/Stat.vue';
import { computed, ref } from 'vue';
import DayCard from './components/DayCard.vue';

const API_ENDPOINT = "http://api.weatherapi.com/v1";

const errorMap = new Map([
  [1006, "Указаный город не найден"]
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
    <div v-if="data" class="day-stat">
      <div>
      <Stat v-for="item in dataModified" v-bind="item" :key="item.label" />
      </div>
      <div class="day-card-list">
        <DayCard 
        v-for="item in data.forecast.forecastday" 
        :key="item.date" 
        :weather-code="item.day.condition.code"
        :temp="item.day.avgtemp_c"
        :date="new Date(item.date)" />
      </div>
    </div>
    <CityCelect @select-city="getCity" />
  </main>
</template>

<style scoped>
.main {
  background-color: var(--color-bg-main);
  padding: 60px 50px;
  border-radius: 25px;
}
.day-card-list{
  display: flex;
}
.day-stat{
  display: flex;
  flex-direction: column;
  gap: 80px;
  margin-bottom: 70px;
}
</style>
