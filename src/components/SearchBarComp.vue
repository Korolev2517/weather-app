<script setup lang="ts">
import { ref, onMounted } from "vue";

const cityName = ref<string | null>(null);
const loading = ref<boolean>(true);
const error = ref<string | null>(null);

onMounted(async () => {
  try {
    const cityResponse = await fetch(
      "http://api.openweathermap.org/geo/1.0/direct?q=Tomsk&limit=5&appid=342874fdf44a9f1f68bf4541ae09ee37",
    );
    if (!cityResponse.ok) {
      throw new Error("Ошибка загрузки данных о городе");
    }
    const data = await cityResponse.json();
    cityName.value = data;
    console.log("Данные о городе:", data);
  } catch (err: any) {
    error.value = err.message;
    console.log("Ошибка:", err);
  } finally {
    loading.value = false;
  }
});
</script>

<template>
  <form action="">
    <input type="text" placeholder="Введите город" />
  </form>
</template>

<style scoped></style>
