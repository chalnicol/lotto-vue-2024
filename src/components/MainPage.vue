<template>
    <div class="w-full max-w-[550px] h-full bg-white relative">

        <Header/>

        <ScheduledGames :schedule="scheduledGamesToday" @showFullSchedule="showFullSchedule"/>

        <SelectForm :options="lottoGames" @generate="startGeneration" :isButtonDisabled="isGenerating"/>
    
        <Combinations :combinations="combinations" />
        

        <Schedule :games="lottoGames" :show="isModalVisible" @close="isModalVisible=false"/>

    </div>

</template>

<script setup>

    import { ref, computed } from 'vue'

    import Header from './Header.vue'
    import SelectForm from './SelectForm.vue'
    import Schedule from './Schedule.vue'
    import Combinations from './Combinations.vue'
    import ScheduledGames from './ScheduledGames.vue'

    const isModalVisible = ref(false);

    const selectedGame = ref(0);

    const combinations = ref([]);

    const showFullSchedule = () => {
        isModalVisible.value = true;
    }

    const startGeneration = (game) => {
        // isGenerating.value = true;
        selectedGame.value = game;
        combinations.value = generateNumbeOfCombinations(6, game);

        // let count = 0;

        // let timer = setInterval( () => {
        //     count++;
        //     console.log("tick...", count);
        //     combinations.value = generateNumbeOfCombinations(6, game);

        //     if (count >= 20 ) {
        //         isGenerating.value = false;
        //         count = 0;
        //         clearInterval(timer);
        //     }
        // }, 2000 );


    }

    const isGenerating = ref(false);

    const lottoGames = ref ([
        { name : 'Lotto', value : '42', schedule : [ 2, 4, 6 ] },
        { name : 'Mega Lotto', value : '45', schedule : [ 1, 3, 5 ] },
        { name : 'Super Lotto', value : '49', schedule : [ 0, 2, 4 ] },
        { name : 'Grand Lotto', value : '55', schedule : [ 1, 3, 6 ] },
        { name : 'Ultra Lotto', value : '58', schedule : [ 0, 2, 5 ] },
    ]);

    const generateCombination = ( max ) => {
        //..
        let arr = Array.from({ length : max }, (_, index) => index + 1);
        
        const shuffled = arr.sort(() => 0.5 - Math.random()); // Shuffle the array

        const selected = shuffled.slice(0, 6);

        return selected.sort((a, b) => a - b); 

    }

    const generateNumbeOfCombinations = ( n, max ) => {
        let arr = [];
        for(let i = 0; i < n; i++) {
            arr.push(generateCombination(max));
        }
        return arr;
    }

    const scheduledGamesToday = computed( () => {
        return lottoGames.value.filter(game => game.schedule.includes(new Date().getDay()));
    });


</script>
