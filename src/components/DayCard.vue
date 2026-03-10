<script setup>
import IconCloud from '../icons/weather/IconCloud.vue';
import IconRain from '../icons/weather/IconRain.vue';
import IconSun from '../icons/weather/IconSun.vue';

const { weatherCode, temp, date } = defineProps({
  weatherCode: Number,
  temp: Number,
  date: Date,

})
</script>

<template>
  <button class="day-card">
    <IconSun v-if="weatherCode === 1000" />
    <IconCloud v-if="weatherCode == 1255" />
    <IconRain v-if="weatherCode == 1198" />
    <div class="day-card__day"> {{ date.toLocaleDateString("ru-RU", { weekday: "short" }) }}</div>
    <div class="day-card__temp"> {{ temp }} C</div>
  </button>
</template>

<style scoped>
.day-card {
  border-radius: 10px;
  border: none;
  cursor: pointer;
  box-shadow: 1px 2px 4px 0px #222831;
  padding: 20px 24px;
  background-color: var(--color-card-bg);
  color: var(--color-primary);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 15px;
  width: 100%;
}

.day-card:hover {
  background-color: #3a434f;

}

.day-card__day {
  font-size: 20px;
  font-weight: 400;
}

.day-card__temp {
  font-size: 20px;
  font-weight: 700;
}
</style>