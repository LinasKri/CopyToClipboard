<template>
  <div class="container my-4">
    <div v-for="(input, index) in inputs" :key="index" class="input-group mb-4">
      <button v-if="input.isCopyButtonLeft" class="rounded-2 me-1 fw-semibold" :class="buttonClasses(index)" type="button"
        @click="copyToClipboard(input.text, index)">
        Copy
      </button>

      <textarea class="form-control rounded-2" v-model="input.text"></textarea>
      <div>

      </div>
      <button v-if="!input.isCopyButtonLeft" class="rounded-2 ms-1" :class="buttonClasses(index)" type="button"
        @click="copyToClipboard(input.text, index)">
        Copy
      </button>

      <div class="d-flex flex-column mx-1" :class="{ 'order-first': !input.isCopyButtonLeft }">
        <button class="btn btn-remove mb-1" type="button" @click="removeInput(index)">Remove</button>
        <button class="btn btn-move" type="button" @click="toggleButtonPosition(index)">
          {{ input.isCopyButtonLeft ? 'Move Right' : 'Move Left' }}
        </button>
      </div>
    </div>
    <button class="btn btn-add" @click="addInput">Add Input</button>
  </div>
</template>

<script>
import { ref, onMounted, watch } from 'vue';

export default {

  setup() {
    const inputs = ref([{ text: '', isCopyButtonLeft: true }]);
    const lastCopiedIndex = ref(-1);

    onMounted(() => {
      const savedInputs = localStorage.getItem('inputs');
      if (savedInputs) {
        inputs.value = JSON.parse(savedInputs);
      }
    });

    // Save to local storage whenever inputs change
    watch(inputs, (newInputs) => {
      localStorage.setItem('inputs', JSON.stringify(newInputs));
    }, { deep: true });

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
        lastCopiedIndex.value === index ? 'btn-pressed' : 'btn-copy'
      ];
    };

    return { inputs, addInput, removeInput, copyToClipboard, toggleButtonPosition, buttonClasses, lastCopiedIndex };
  },
};
</script>

<style>
.btn-copy {
  background-color: #fec84d;
}

.btn-copy:hover {
  background-color: #f4d180;
}

.btn-add {
  background-color: #004369;
  color: #fff;
}

.btn-add:hover {
  background-color: #36596e;
  color: #fff;
}

.btn-move {
  background-color: #01948a;
  color: #fff;
}

.btn-move:hover {
  background-color: #619592;
  color: #fff;
}

.btn-remove {
  background-color: #db1f48;
  color: #fff;
}

.btn-remove:hover {
  background-color: #da637c;
  color: #fff;
}

.btn-pressed {
  background-color: #4fb3c7;
}

.btn-pressed:hover {
  background-color: #69adbb;
}
</style>