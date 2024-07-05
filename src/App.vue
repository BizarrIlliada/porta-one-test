<template>
  <FileInputComponent class="file-input" @onFileAdded="calculateValues" />

  <div class="info">
    Min: {{ values.min }}
    <br>
    Max: {{ values.max }}
    <br>
    Average: {{ values.average }}
    <br>
    Median: {{ values.median }}
  </div>
</template>

<script setup lang="ts">
import FileInputComponent from '@/components/FileInputComponent.vue';

import { reactive } from 'vue';

const values = reactive({
  min: 0,
  max: 0,
  average: 0,
  median: 0,
});

function calculateValues(numbers: number[]) {
  let min = numbers[0];
  let max = numbers[0];
  let sum = 0;

  for (let i = 0; i < numbers.length; i++) {
    if (numbers[i] < min) {
      min = numbers[i];
    }

    if (numbers[i] > max) {
      max = numbers[i];
    }

    sum += numbers[i];
  }

  values.min = min;
  values.max = max;
  values.average = +(sum / numbers.length).toFixed(2);
  values.median = numbers.length % 2 === 0
    ? 0.5 * (+numbers[(numbers.length / 2) - 1] + +numbers[(numbers.length / 2)])
    : +numbers[Math.floor(numbers.length / 2)];
}
</script>

<style scoped lang="scss">
.file-input {
  margin-bottom: 40px;
}
</style>
