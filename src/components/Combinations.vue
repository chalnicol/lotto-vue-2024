<template>
  <div v-if="combinations.length > 0">
    <div class="mt-5 md:mt-12">

      <div v-for="(combination, i) in combinations" :key="i" class="flex items-center justify-center gap-5 md:gap-6 mb-3 md:mb-6 select-none w-full">
          <Circle v-for="(el, j) in combination" :key="j" :val="el" :blink="blink[i][j]"/>
      </div>

    </div>
    <hr />

    <div class="flex items-center justify-center gap-5 md:gap-6 mb-3 md:mb-6 select-none w-full">
          <Circle v-for="(fin, k) in finCombination" :key="k" :val="fin"/>
      </div>

   
    
  </div>
  <div v-else>
    <div
      class="w-[90%] text-center mx-auto border border-gray-300 my-5 px-3 py-4 bg-gray-200 rounded-md shadow-lg"
    >
      <p class="text-sm md:text-base font-medium">
        Select desired game and hit
        <span class="px-2 mx-1 py-0.5 bg-blue-500 text-white rounded"
          >Generate</span
        >
        to start.
      </p>
    </div>
  </div>
</template>
  
<script setup>
import { computed, defineProps, onMounted, ref, watch, watchEffect } from "vue";
import Circle from "./Circle.vue";
import { gsap } from "gsap";

// Define the props
const props = defineProps({
  combinations: {
    type: Array,
    required: true,
  },
});

const generateClass = ( row, col ) => {
  return `circle circle_${row}_$(col)`;
}

const finCombination = computed ( () => {

  let mostIdenticalNumbers = [];

  for (let col = 0; col < props.combinations[0].length; col++) {
    
      let numberCount = {};
      let maxCount = 0;
      let maxNumber = "";

      for (let row = 0; row < props.combinations.length; row++) {
          let number = props.combinations[row][col];
          numberCount[number] = (numberCount[number] || 0) + 1;

          if (numberCount[number] > maxCount) {
              maxCount = numberCount[number];
              maxNumber = number;
          }
      }

      // Only consider maxNumber if it appears more than once
      mostIdenticalNumbers[col] = maxCount > 1 ? maxNumber : 0;
  }

  return mostIdenticalNumbers;

});

const getIdenticalOccurences = ( ( matrix ) => {

  const numRows = matrix.length;
  const numCols = matrix[0].length;
  const result = [];

  // Loop through each column
  for (let col = 0; col < numCols; col++) {

    const frequency = {};

    // Loop through each row in the column to build the frequency map
    for (let row = 0; row < numRows; row++) {
        const element = matrix[row][col];
        if (!frequency[element]) {
            frequency[element] = { count: 0, indexes: [] };
        }
        frequency[element].count += 1;
        frequency[element].indexes.push([row, col]);
    }

    // Determine the element with the highest frequency in this column
    let maxFrequency = 0;
    let mostFrequentElement = null;

    for (const element in frequency) {
        if (frequency[element].count > maxFrequency) {
            maxFrequency = frequency[element].count;
            mostFrequentElement = element;
        }
    }

    // Store the indexes only if there are duplicates
    if ( maxFrequency > 1 ) {
      for ( let i in frequency[mostFrequentElement].indexes ) {
        result.push ( frequency[mostFrequentElement].indexes[i] ); 
      }
    }
    // result.push(maxFrequency > 1 ? frequency[mostFrequentElement].indexes : [] );

  }

  return result;

})

const blink = computed ( () => {

  let fin = Array(6).fill(null).map(() => Array(6).fill(false));

    if ( props.combinations.length > 0 ) {
      //
      let posts = getIdenticalOccurences ( props.combinations );

      for ( let i in posts ) {
        let [row, col] = posts[i];
        fin[row][col] = true;
      }

    }

    return fin;
});




  




</script>