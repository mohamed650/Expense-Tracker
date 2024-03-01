<template>
    <h4>Your Balance</h4>
    <h1 id="balance">${{total}}</h1>
</template>

<script setup>
import { defineProps } from 'vue';

// Define props for the component using defineProps
const props = defineProps({
    // 'total' prop represents the total value
    total: {
        type: Number,
        required: true
    }
})
</script>