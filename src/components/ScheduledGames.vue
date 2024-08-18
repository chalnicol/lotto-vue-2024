<template>

    <div class="w-[90%] mx-auto mt-3">

        <div class="w-full rounded-md overflow-hidden shadow-md border border-gray-300">

            <div class="font-bold text-sm md:text-base bg-blue-500 text-white px-3 py-1 flex justify-between items-center">
              <div class="text-base md:text-lg">Games Today</div>
             
                <button class="font-medium hover:text-blue-100 py-1 rounded-md text-white text-xs md:text-sm cursor-pointer active:scale-95" @click="btnClick">See Full Schedule</button>
            
            </div>

            <div class="flex gap-3 md:gap-5 py-3 bg-gray-200 grow justify-center">

                <div v-for="sched in schedule" :key="sched" class="flex flex-col items-center">
                    <div class="text-sm md:text-base flex justify-center items-center font-bold w-12 h-12 md:w-14 md:h-14 rounded-full border border-gray-500 bg-white shadow-lg">6/{{sched.value}}</div>
                    <div class="text-xs md:text-sm text-gray-600 mt-1">{{ sched.name }}</div>
                </div>
                
            </div>

            
        </div>
        <!-- <div class="w-full mt-2">
            <button class="w-full font-medium bg-gray-600  hover:bg-gray-400  py-1 px-4 rounded-md text-white text-sm md:text-base cursor-pointer active:scale-95" @click="btnClick">See Full Schedule</button>
        </div> -->
    </div>

</template>
  
<script setup>
  import { defineProps, defineEmits } from 'vue';
  
  // Define the props
  const props = defineProps({
    schedule : {
      type: Array,
      required: true,
    },
  });

  const emit = defineEmits(['showFullSchedule']);

  const btnClick = () => {
    emit('showFullSchedule');
  }

</script>