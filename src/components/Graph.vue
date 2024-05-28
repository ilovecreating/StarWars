<template>
  <div>
    <div class="chart-container">
      <div class="chart-column">
        <div class="chart" v-for="item in info" :key="item.name">
          <div class="label">{{ item.name }}</div>
        </div>
      </div>
      <div class="bar-row">
        <div class="bar-elem" v-for="item in info" :key="item.height">
          <div class="bar" :style="{ height: item.height + 'px' }">{{}}</div>
        </div>
        <div class="bar-row-number">
          <div class="bar-foo" v-for="item in info" :key="item.height">
            <div>{{ item.height }}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref, computed } from 'vue';
import axios from 'axios';

const info = ref<any[]>([]);

onMounted(async () => {
  try {
    const response = await axios.get('https://swapi.dev/api/people/');
    info.value = response.data.results;
  } catch (err) {
    console.log(err);
  }
});
</script>

<style scoped>
.chart-container {
  align-items: flex-end;
  display: flex;

  height: 100%;
}

.chart {
  display: flex;
  align-items: center;
  padding-top: 5px;
}
.chart-column {
  margin-bottom: 20px;
  margin-right: 15px;
}
.bar {
  background-color: #3498db;
  width: 50px;
}

.bar-elem {
  display: inline-block;
  margin-right: 2px;
}
.bar-row-number {
  display: flex;
  gap: 30px;
}

.bar-row {
}
.bar-foo {
  display: flex;
}
.label {
  margin-left: 10px;
}
</style>
