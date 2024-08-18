<template>

  <div class="flex items-center justify-center gap-2 text-sm sm:text-base mt-5 w-[90%] mx-auto">
    <div class="grow">
        
        <select v-model="selected" class="py-2 px-1 w-full border border-gray-400 rounded-md"
        >
        <option v-for="(data, index) in options" :key="index" :value="data.value">
            6/{{ data.value }} {{ data.name }} 
        </option>
        </select>
    </div>
    <div>
      <button class="bg-blue-500 py-2 px-4 rounded-lg text-white hover:bg-blue-400 cursor-pointer active:scale-95" @click="generateClick" :disabled="isButtonDisabled">Generate</button>
    </div>
  </div>
</template>
  
<script setup>
  import { ref, defineProps, defineEmits } from 'vue';
  
  // Define the props
  const props = defineProps({
    options: {
      type: Array,
      required: true,
    },
    isButtonDisabled : {
      type: Boolean,
      default: false,
    }
  });

  const selected = ref(42);
  
  
  // Define the emits
  const emit = defineEmits(['generate']);
  
  // Function to emit the selected value
  function generateClick () {
    emit('generate', selected.value);
  }
  </script>