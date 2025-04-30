<script setup>
import { computed } from 'vue';

const props = defineProps({
    quiz: Array,
    answers: Array
})

const score = computed(() => {
    return props.quiz.reduce((acc, quiz, k) => {
        // const correctanswer = quiz[quiz.answer];
        const correctanswer = quiz.answer;
         const resutat = quiz[correctanswer]
    
        if (resutat === props.answers[k]) {
            return acc + 1;
        }
        return acc
    }, 0)
})
const hasWon = computed(()=> {
    if(score.value >= 5){
        return "Tu a ganier"
    }
    else{
        return "Tu n'a pas la moyenne "
    }
}) 
</script>

<template>
    <h1>Recap</h1>
    <p>
        Score : {{ score }}/{{ quiz.length }}
    </p>
    <p>
        {{ hasWon }}
    </p>

</template>
