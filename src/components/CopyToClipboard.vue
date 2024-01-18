<template>
  <div class="container my-4">
    <div v-for="(input, index) in inputs" :key="index" class="input-group mb-3">
      <button v-if="input.isCopyButtonLeft" :class="buttonClasses(index)" type="button"
        @click="copyToClipboard(input.text, index)">
        Copy
      </button>

      <textarea class="form-control" v-model="input.text"></textarea>

      <button v-if="!input.isCopyButtonLeft" :class="buttonClasses(index)" type="button"
        @click="copyToClipboard(input.text, index)">
        Copy
      </button>

      <div :class="{ 'order-first': !input.isCopyButtonLeft }">
        <button class="btn btn-danger" type="button" @click="removeInput(index)">Remove</button>
        <button class="btn btn-secondary" type="button" @click="toggleButtonPosition(index)">
          {{ input.isCopyButtonLeft ? 'Move Right' : 'Move Left' }}
        </button>
      </div>
    </div>
    <button class="btn btn-primary" @click="addInput">Add Input</button>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const inputs = ref([{ text: '', isCopyButtonLeft: true }]);
    const lastCopiedIndex = ref(-1);

    const addInput = () => {
      inputs.value.push({ text: '', isCopyButtonLeft: true });
    };

    const removeInput = (index) => {
      inputs.value.splice(index, 1);
      if (index === lastCopiedIndex.value) {
        lastCopiedIndex.value = -1;
      }
    };

    const copyToClipboard = async (text, index) => {
      try {
        await navigator.clipboard.writeText(text);
        lastCopiedIndex.value = index;
      } catch (err) {
        console.error('Failed to copy: ', err);
      }
    };

    const toggleButtonPosition = (index) => {
      inputs.value[index].isCopyButtonLeft = !inputs.value[index].isCopyButtonLeft;
    };

    const buttonClasses = (index) => {
      return [
        'btn',
        lastCopiedIndex.value === index ? 'btn-info' : 'btn-warning'
      ];
    };

    return { inputs, addInput, removeInput, copyToClipboard, toggleButtonPosition, buttonClasses, lastCopiedIndex };
  },
};
</script>

<style>
* {
  background-color: rgb(28, 113, 85);
}
</style>