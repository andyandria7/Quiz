<script setup>
defineProps({
    value: Number,
    max: Number
})
</script>

<template>
    <p>
        Etat: {{ value }}/{{ max }}
    </p>
    <progress :value="value" :max="max"/>
</template>