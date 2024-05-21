<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpensess :income="+income" :expense="+expense" />
    <TransactionHistory :transactionData="transactionData" @transactionDeletted="handleTransactionDeletted" />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>


</template>

<script setup>
import Header from './components/Header.vue'
import Balance from './components/Balance.vue'
import IncomeExpensess from './components/IncomeExpensess.vue'
import TransactionHistory from './components/TransactionHistory.vue'
import AddTransaction from './components/AddTransaction.vue'
import { ref, computed, onMounted } from 'vue'

onMounted(() => {
  const saveTransaction = JSON.parse(localStorage.getItem('transactions'))

  if (saveTransaction) {
    transactionData.value = saveTransaction
  }
})

const transactionData = ref([
  { id: 1, text: 'EMI', amount: -200 },
  { id: 2, text: 'Rent', amount: -50 },
  { id: 3, text: 'Salary', amount: 600 }
])

// get total
const total = computed(() => {
  return transactionData.value
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0)
})

// get income
const income = computed(() => {
  return transactionData.value
    .filter((transactionData) => transactionData.amount > 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2)
})

// get income
const expense = computed(() => {
  return transactionData.value
    .filter((transactionData) => transactionData.amount < 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2)
})

// get
const handleTransactionSubmitted = (transaction) => {
  transactionData.value.push({
    id: generateId(),
    text: transaction.text,
    amount: transaction.amount
  }),
  saveToLocalStorage()
}

// get id
const generateId = () => {
  return Math.floor(Math.random() * 100000)
}

// delete Transaction
const handleTransactionDeletted = (id) => {
  transactionData.value = transactionData.value.filter(
    (transactionData) => transactionData.id !== id
  )
  saveToLocalStorage()
}

// save to local storage
const saveToLocalStorage = () => {
  localStorage.setItem('transactions' , JSON.stringify(transactionData.value))
}
</script>