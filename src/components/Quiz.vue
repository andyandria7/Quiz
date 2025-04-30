<script setup>
import { shuffleArray } from '@/function/array';
import { computed, onMounted, ref, watch } from 'vue';
import Answer from './Answer.vue';


const props = defineProps({
    quiz: Object
})

const emits = defineEmits(['answer'])
const answer = ref(null)
const hasAnswer = computed(() => answer.value !== null);



const shuffledAnswers = ref([]);
const shuffleAnswers = () => {
  const answersArray = [
    { text: props.quiz.A, value: 'A' },
    { text: props.quiz.B, value: 'B' },
    { text: props.quiz.C, value: 'C' },
    { text: props.quiz.D, value: 'D' }
  ];
  shuffledAnswers.value = shuffleArray(answersArray);
};

// Appeler shuffleAnswers chaque fois que la question change
watch(() => props.quiz, () => {
  shuffleAnswers();
}, { immediate: true });

const handleAnswer = () => {
  emits('answer', answer.value);
  answer.value = null;
};
</script>

<template>
    <div class="question">
        <h2>{{ quiz.question }}</h2>
        <ul>
            <li v-for="(ans, index) in shuffledAnswers" :key="index">
                <Answer
                :for="`answer${index}`"
                :id="`answer${index}`"
                :value="ans.text"
                v-model="answer"
                :correctAnswer="quiz.answer"
                :disabled="hasAnswer"
                />
                <!-- <label :for="`answer${index}`">
                    <input :id="`answer${index}`" type="radio" name="answer" v-model="answer" :value="ans.text">
                    {{ ans.text }}
                </label> -->
            </li>
        </ul>
        <button v-if="hasAnswer" class="btn-grad" @click="handleAnswer">
            Question suivante
        </button>
    </div>
</template>

<style>
.btn-grad {
    background-image: linear-gradient(to right, #FF512F 0%, #F09819 51%, #FF512F 100%);
    margin: 10px;
    padding: 15px 45px;
    text-align: center;
    text-transform: uppercase;
    transition: 0.5s;
    background-size: 200% auto;
    color: white;
    box-shadow: 0 0 20px #eee;
    border-radius: 10px;
    display: block;
}

.btn-grad:hover {
    background-position: right center;
    /* change the direction of the change here */
    color: #fff;
    text-decoration: none;
}

.question .btn-grad{
    margin-left: auto;
    display: block;
}
</style>