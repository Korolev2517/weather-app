<script setup lang="ts">
import { ref, onMounted } from "vue";
import SearchBarComp from "./components/SearchBarComp.vue";

interface OpenWeatherResponse {
  main: {
    temp: number;
    feels_like: number;
    humidity: number;
  };
  weather: Array<{
    main: string;
    description: string;
    icon: string;
  }>;
  name: string;
}

const weatherData = ref<OpenWeatherResponse | null>(null);
const loading = ref<boolean>(true);
const error = ref<string | null>(null);

onMounted(async () => {
  try {
    const weatherResponse = await fetch(
      "http://api.openweathermap.org/data/2.5/weather?q=London,uk&lang=ru&units=metric&APPID=342874fdf44a9f1f68bf4541ae09ee37",
    );
    if (!weatherResponse.ok) {
      throw new Error("Ошибка загрузки данных о погоде");
    }
    const data: OpenWeatherResponse = await weatherResponse.json();
    weatherData.value = data;
    console.log("Данные о погоде:", data);
  } catch (err: any) {
    error.value = err.message;
    console.log("Ошибка:", err);
  } finally {
    loading.value = false;
  }
});
</script>

<template>
  <SearchBarComp></SearchBarComp>
</template>

<style scoped></style>
