<template>

    <div v-if="isVisible" class="fixed w-full h-full max-w-[550px] flex items-center justify-center top-0"> 

        <div class="absolute w-full h-full bg-black opacity-[0.8]" @click="closeModal"></div>

        <div ref="modalContent" class="absolute w-[90%] mx-auto border px-3 pt-2 pb-3 shadow-md rounded-md select-none bg-white">

            <h1 class="font-bold mt-1 mb-4 text-xl md:text-2xl">Lotto Games Schedule</h1>

            <div v-for="(day, index) in days" :key="index" class="flex mb-2">
                <div class="w-1/3 font-bold bg-gray-200 mr-2 p-2 rounded-sm text-sm md:text-lg">{{ day }}</div>

                <div class="flex gap-2 md:gap-3 lg:gap-5 p-1 md:p-2 grow" :class="{  'bg-blue-300': index === currentDate  }">

                    <div class="flex flex-col items-center" v-for="sched in scheduledGames[index]" :key="sched">
                        <div class="font-bold border border-gray-500 w-12 h-12 md:w-16 md:h-16 text-base md:text-xl  bg-white shadow-md rounded-full mr-1 flex justify-center items-center">6/{{ sched.value }}</div>
                        <div class="text-xs md:text-base md:mt-1 text-gray-500">{{ sched.name }}</div>
                    </div>
                </div>
            </div>

            <div class="border  border-gray-500 shadow-md rounded-full w-6 h-6 md:w-8 md:h-8 bg-white absolute top-3 right-3 md:top-4 md:right-4 flex justify-center items-center font-bold hover:bg-gray-200 cursor-pointer select-none text-base md:text-xl" @click="closeModal">&#10006;</div>

        </div>

      
    </div>
</template>
  
<script setup>
  import { ref, computed, defineProps, defineEmits, watch } from 'vue';
  import { gsap } from "gsap";


  const props = defineProps({
    games : {
        type: Array,
        required: true
    },
    show: {
      type: Boolean,
      default: false
    }
  });

  const currentDate = ref(new Date().getDay());

  const modalContent = ref(null);

  const emit = defineEmits(['close']);

  const isVisible = ref(props.show);

  const animateOpen = () => {
    // Open the schedule modal here
    gsap.fromTo(modalContent.value, { scale:0 }, { scale : 1, duration: 0.8, opacity: 1, ease: 'elastic.out(1, 0.9)' });

  }

  const animateClose = () => {
    // Close the schedule modal here
    gsap.fromTo(modalContent.value, { scale:1 }, { scale : 0, duration: 0.8, opacity: 0, ease: 'elastic.in(1, 0.9)', onComplete: () => emit('close') });

  };

  const closeModal = () => {
    animateClose();
  };


  watch(() => props.show, (newValue) => {
    isVisible.value = newValue;
    if (newValue) {
      // gsap.delayedCall(0.3, ()=> animateOpen() )
    }
  });


  const days = ref(['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']);

  const scheduledGames = computed(() => {
  // Create an array of empty arrays for each day of the week
  const schedArr = Array.from({ length: 7 }, () => []);

  // Iterate over each game and populate the schedArr
  props.games.forEach(game => {
    game.schedule.forEach(day => {
      schedArr[day].push({
        name: game.name,
        value: game.value
      });
    });
  });

  return schedArr;
});



  
</script>