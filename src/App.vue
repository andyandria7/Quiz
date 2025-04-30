
<script setup>
import { computed, onMounted, ref } from 'vue';
import Quiz from './components/Quiz.vue';
import Progress from './components/Progress.vue';
import Recap from './components/Recap.vue';

const quiz = ref([])
const state = ref('loading')
const step = ref(0)

onMounted(()=>{
  fetch('/questions.json')
  .then(r => r.json())
  .then(v => {
    quiz.value = v,
    answers.value = v.map(() => null)
    state.value = 'ready'
  })
  .catch(e =>{
    state.value = 'error'
  })
})

const questionnaire = computed(() => quiz.value[step.value])
const answers = ref(quiz.value.map(()=>null))
const question = ref('question')

const addAnswer = (answer) =>{
  answers.value[step.value] = answer
  if(step.value === quiz.value.length - 1){
    question.value = 'recap'
  } else{
    step.value++
  }
}

</script>

<template>
  <div>
    <Progress :value="step" :max="quiz.length"/>
  </div>
  <div v-if="state === 'error'">
    Impossible de charger les questions.
  </div>
  
  <div v-else-if="state === 'loading'">
    Chargement des questions...
  </div>
  <div v-else>
    <Quiz :quiz="questionnaire" v-if="question === 'question'" @answer="addAnswer"/>
    <Recap v-if="question === 'recap'" :quiz="quiz" :answers="answers"/>
  </div>
</template>