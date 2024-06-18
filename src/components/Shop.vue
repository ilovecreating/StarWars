<script setup lang="ts">
import { onMounted, ref, computed } from 'vue';
import axios from 'axios';

const info = ref<any[]>([]);
const goods = ref<number>(1)
const currentPage = ref<number>(1);
const allPages = ref<number>(0);
const itemsPerPage: number = 40;



const products = ref([
  { id: 1, title: 'Product 1' },
  { id: 2, title: 'Product 2' },
  { id: 3, title: 'Product 3' },
  { id: 4, title: 'Product 4' },
  { id: 5, title: 'Product 5' },
  { id: 6, title: 'Product 6' },
  { id: 7, title: 'Product 7' },
  { id: 8, title: 'Product 8' },
  { id: 9, title: 'Product 9' },
  { id: 10, title: 'Product 10' },
  { id: 11, title: 'Product 11' },
  { id: 12, title: 'Product 12' },
  { id: 13, title: 'Product 13' },
  { id: 14, title: 'Product 14' },
  { id: 15, title: 'Product 15' },
  { id: 16, title: 'Product 16' },
  { id: 17, title: 'Product 17' },
  { id: 18, title: 'Product 18' },
])
const selectedProduct = ref<number>(0)

const goToProduct = (id:number) => {
  selectedProduct.value = id
}






const loadProducts = async (count:number) => {
  try {
    const response = await fetch(`https://fakestoreapi.com/products?limit=${count}`)
    const data = await response.json()
    info.value = data
  } catch (error) {
    console.error(error)
  }
}
loadProducts(8)


const loadAllProducts = async () => {
  try {
    const response = await axios.get('https://fakestoreapi.com/products');
    info.value = response.data;
    allPages.value = Math.ceil(response.data.count / itemsPerPage);
    console.log(info.value);
  } catch (err) {
    console.log(err);
  }
};

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
<div class="good" v-for="good in goods" :key="good">
  <div v-for="item in info" :key="item.id" class="card" @click="goToProduct(product.id)">
    <p class="rectangle">
      <div>name: {{ item.title }}</div>
      <div>price {{ item.price }}</div>
      <img :src="item.image" alt="image"/> 
    </p>
  </div>
  </div>
  <button @click="loadProducts(8)">Загрузить 8 продуктов</button>
    <button @click="loadProducts(16)">Загрузить 16 продуктов</button>
    <button @click="loadAllProducts">Загрузить все продукты</button>
    <router-link to="/product" v-if="selectedProduct !== null">Перейти к выбранному продукту</router-link>
</template>

<style scoped>

.good{
  display: flex;
  flex-wrap: wrap; 
}

.card {
  display: flex;
  flex-wrap: wrap; /* Создает четыре колонки равной ширины */
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
  width: 200px;

}
img{
  width: 100%;
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
