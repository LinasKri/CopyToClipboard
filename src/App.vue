<template>
  <main :style="{ backgroundColor: backgroundColor }">
    <h1 class="text-center text-white pt-3 mb-3">Copy To Clipboard</h1>
    <CopyToClipboard />

    <div class="color-picker-container text-center d-flex justify-content-center align-items-center my-3">
      <label for="bg-color-picker" class="form-label text-white me-2 mb-0">Select Background Color:</label>
      <input id="bg-color-picker rounded-2" type="color" v-model="backgroundColor" />
      <button class="btn-reset-color ms-2" @click="resetBackgroundColor">&#10227;</button>
    </div>
  </main>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue';
import CopyToClipboard from './components/CopyToClipboard.vue';

const defaultColor = '#013a20';
const backgroundColor = ref(defaultColor);

onMounted(() => {
  document.title = "COPY!";
  const savedColor = localStorage.getItem('backgroundColor');
  if (savedColor) {
    backgroundColor.value = savedColor;
  }
});

watch(backgroundColor, (newValue) => {
  localStorage.setItem('backgroundColor', newValue);
});

const resetBackgroundColor = () => {
  backgroundColor.value = defaultColor;
  localStorage.setItem('backgroundColor', defaultColor);
};
</script>

<style>
.btn-reset-color {
  border-radius: 20%;
  font-size: large;
}

.color-picker-container {
  margin-top: 20px;
}
</style>

<style scoped>
main {
  min-height: 100vh;
}

.color-picker-container {
  margin-top: 20px;
}
</style>