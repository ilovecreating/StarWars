<script setup lang="ts">
import { onMounted, ref, computed } from 'vue';
import axios from 'axios';

const info = ref<any[]>([]);
const currentPage = ref<number>(1);
const allPages = ref<number>(0);
const itemsPerPage: number = 40;
onMounted(async () => {
  try {
    const response = await axios.get('https://swapi.dev/api/people/');
    info.value = response.data.results;
    allPages.value = Math.ceil(response.data.count / itemsPerPage);
    console.log(allPages.value);
  } catch (err) {
    console.log(err);
  }
});

defineProps<{ msg: string }>();

const paginatedItems = computed((): any => {
  const startIndex = (currentPage.value - 1) * itemsPerPage;
  const endIndex = currentPage.value * itemsPerPage;
  return info.value.slice(startIndex, endIndex);
});

const previousPage = (): void => {
  if (currentPage.value > 1) currentPage.value--;
};

const nextPage = (): void => {
  if (currentPage.value < allPages.value) currentPage.value++;
};
</script>

<template>
  <h1>{{ msg }}</h1>

  <div v-for="item in paginatedItems" :key="item.url" class="card">
    <p class="rectangle">name: {{ item.name }}</p>
    <p class="rectangle">height: {{ item.height }}</p>
    <p class="rectangle">birth_year: {{ item.birth_year }}</p>
    <p class="rectangle">gender: {{ item.gender }}</p>
  </div>
  <button @click="previousPage" :disabled="currentPage <= 1">Previous</button>
  <span>Page {{ currentPage }}</span>
  <button @click="nextPage" :disabled="currentPage >= allPages">Next</button>
</template>

<style scoped>
.card {
  display: grid;
  grid-template-columns: repeat(4, 1fr); /* Создает четыре колонки равной ширины */
  gap: 10px;
}

.rectangle {
  border-radius: 8px;
  border: 1px solid #000; /* Добавляем границу черного цвета шириной 1 пиксел */
  padding: 0.6em 1.2em;
  font-size: 1em;
  font-weight: 500;
  font-family: inherit;
  transition: border-color 0.25s;
}
.rectangle:hover {
  border-color: #646cff;
}
.rectangle:focus,
.rectangle:focus-visible {
  outline: 4px auto -webkit-focus-ring-color;
}

.read-the-docs {
  color: #888;
}
</style>
