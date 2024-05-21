<template>
    <h4>YOUR BALANCE</h4>
    <h1>${{total}}</h1>
</template>

<script setup>

import { defineProps } from 'vue';

const props = defineProps({
    total:{
        type:Number,
        required: true
    }
})
</script>