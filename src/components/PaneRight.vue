<script setup>
import Error from "./Error.vue";
import DayCard from "./DayCard.vue";
import CityCelect from "./CityCelect.vue";
import Stat from "./Stat.vue";
import { computed } from "vue";
const { error, data, activeIndex } = defineProps({
  error: Object,
  data: Object,
  activeIndex: Number,
});

const emit = defineEmits(["select-index", "select-city"]);

const errorMap = new Map([[1006, "Указаный город не найден"]]);

const statData = computed(() => {
  if (!data) {
    return [];
  }
  return [
    {
      label: "Влажность",
      stat: `${data.current.humidity}%`,
    },

    {
      label: "Облачность",
      stat: `${data.current.cloud}%`,
    },

    {
      label: "Ветер",
      stat: `${data.current.wind_kph}км/ч`,
    },
  ];
});

const errorDisplay = computed(() => {
  return errorMap.get(error?.error?.code);
});
</script>

<template>
  <div>
    <Error :error="errorDisplay" />
    <div v-if="data" class="day-stat">
      <div>
        <Stat v-for="item in statData" v-bind="item" :key="item.label" />
      </div>
      <div class="day-card-list">
        <DayCard
          v-for="(item, index) in data.forecast.forecastday"
          :key="item.date"
          :weather-code="item.day.condition.code"
          :temp="item.day.avgtemp_c"
          :date="new Date(item.date)"
          :is-active="activeIndex == index"
          @click="() => emit('select-index', i)"
        />
      </div>
    </div>
    <CityCelect @select-city="(city) => emit('select-city', city)" />
  </div>
</template>

<style scoped>
.day-card-list {
  display: flex;
}
.day-stat {
  display: flex;
  flex-direction: column;
  gap: 80px;
  margin-bottom: 70px;
}
</style>
