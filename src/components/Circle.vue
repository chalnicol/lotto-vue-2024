
<template>
  <div ref="el" class="w-[10%] md:w-[11%] h-auto aspect-square bg-white flex justify-center items-center rounded-full border border-gray-500 text-base md:text-2xl font-bold shadow-xl" 
  >
    {{ lessTen ( val ) }}
  </div>
</template>

<script setup>

import { ref, watch, defineProps, onMounted, watchEffect } from 'vue'
import { gsap } from 'gsap';

const props = defineProps({
    val: {
        type: Number,
        required: true,
        default: 0
    },
    blink : {
        type: Boolean,
        default: false
    }
});

const lessTen = (n) => {
    if ( n > 0 ) {
        return n >= 10 ? n : "0" + n;
    }
    return '-';
};

const el = ref(null);
const isMounted = ref(false);

onMounted ( () => {
    // console.log ( "onMounted");
    isMounted.value = true;
});

    
watchEffect ( () => {

    // console.log("watchEffect");

    if (isMounted.value) {

        gsap.killTweensOf(el.value);

        gsap.set ( el.value, { backgroundColor: '#fff' });

        if ( props.blink ) {

            gsap.fromTo (el.value, { backgroundColor: '#fff'}, { backgroundColor:'#00c30066', ease : 'linear', yoyo: true, repeat: -1, duration: 0.4, delay:0.3 });
        }
    }
});




</script>



<style scoped>
</style>
