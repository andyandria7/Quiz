<script setup>
import { computed } from 'vue';

const props = defineProps({
    id: String,
    value: String,
    disabled: Boolean,
    correctAnswer: String,
    modelValue: String // Utilisé pour v-model
});

const emit = defineEmits(['update:modelValue']); // Émettre l'événement pour v-model

// Calculer les classes à appliquer en fonction de l'état
const classes = computed(() => ({
    right: props.disabled && props.value === props.correctAnswer, // Bonnes réponses
    wrong: props.disabled && props.value !== props.correctAnswer && props.value === props.modelValue // Mauvaises réponses
}));

// Mettre à jour le modèle lorsque la réponse est sélectionnée
const updateModelValue = () => {
    emit('update:modelValue', props.value); // Émettre l'événement avec la valeur actuelle
};
</script>

<template>
    <label :for="id" :class="classes">
        <input
            :disabled="disabled"
            :id="id"
            type="radio"
            name="answer"
            :value="value"
            @change="updateModelValue" 
        />
        {{ value }} <!-- Affiche la valeur de la réponse -->
    </label>
</template>

<style>
.right {
    color: green; /* Couleur pour la bonne réponse */
}

.wrong {
    color: red; /* Couleur pour la mauvaise réponse */
}
</style>
