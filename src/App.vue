<script setup>
import CopyToClipboard from './components/CopyToClipboard.vue'

import { onMounted } from 'vue';

onMounted(() => {
  document.title = "COPY!";
});
</script>

<template>
  <main>
    <h1 class="text-center text-white">Copy To Clipboard</h1>
    <CopyToClipboard />
  </main>
</template>

