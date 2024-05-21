<template>
    <h3>History</h3>
    <ul id="list" class="list">
        <li v-for="transaction in transactionData" :class="transaction.amount > 0 ? 'plus' : 'minus'">
            {{ transaction.text }}
            <span>{{ transaction.amount }}</span>
            <button class="delete-btn" @click="handleDelete(transaction.id)">x</button>
        </li>
    </ul>
</template>
<script setup>
import { defineProps } from 'vue';

const emit = defineEmits(['transactionDeletted'])
const props = defineProps({
    transactionData: {
        type: Array,
        required: true
    }
})

const handleDelete = (id) => {
    emit('transactionDeletted', id)
}

</script>