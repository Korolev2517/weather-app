<script setup lang="ts">
import { ref, onMounted } from "vue";

interface HourlyUnits {
  time: string;
  temperature_2m: string;
}

interface HourlyData {
  time: string[];
  temperature_2m: number[];
}

interface WeatherResponse {
  generationtime_ms: number;
  utc_offset_seconds: number;
  timezone: string;
  timezone_abbreviation: string;
  elevation: number;
  hourly_units: HourlyUnits;
  hourly: HourlyData;
}

const weatherData = ref<WeatherResponse | null>(null);
const loading = ref<boolean>(true);
const error = ref<string | null>(null);

onMounted(async () => {
  try {
    const response = await fetch(
      "https://api.open-meteo.com/v1/forecast?latitude=56.5003&longitude=84.982&hourly=temperature_2m&forecast_days=1",
    );
    if (!response.ok) {
      throw new Error("Ошибка загрузки данных");
    }
    const data = (await response.json()) as WeatherResponse;
    weatherData.value = data;
    console.log("Данные:", data);
  } catch (err: any) {
    error.value = err.message;
    console.log("Ошибка:", err);
  } finally {
    loading.value = false;
  }
});
</script>

<template>
  <div v-if="loading">Загрузка...</div>
  <div v-else-if="error">Ошибка: {{ error }}</div>
  <div v-else>
    <h3>Температура на сегодня:</h3>
    <p v-if="weatherData?.hourly?.temperature_2m">
      Сейчас: {{ weatherData.hourly.temperature_2m[0] }}°C
    </p>
  </div>
</template>

<style scoped></style>
