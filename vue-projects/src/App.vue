<template>
  <Header></Header>
  <div class="container">
    <balance :total="total" />
    <IncomeExpense :income="income" :expense="expense"/>
    <Transactions :transactions="transactions" @transactionDeleted="handletranactiondeleted"/>
    <AddTransaction @transactionsubmitted="handleTransactionSubmitted"/>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import Header from './components/header.vue';
import balance from './components/balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import Transactions from './components/Transactions.vue';
import AddTransaction from './components/AddTransaction.vue';
import { useToast } from 'vue-toastification';

const toast=useToast();

const transactions = ref([
  { id: 1, text: 'Flower', amount: -50.11 },
  { id: 2, text: 'Salary', amount: 1000 },
  { id: 3, text: 'Book', amount: -10 },
  { id: 4, text: 'Camera', amount: -200.11 },
]);

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) =>
    acc + transaction.amount, 0
  );
});

//get income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0);
});

//get expense
const expense = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0);
});

const handleTransactionSubmitted= (transactionData)=>{
  transactions.value.push({
    id:generateId(),
    text:transactionData.text,
    amount:transactionData.amount
  })
  toast.success("Transaction Added Successfully");
  
}

//Generate Unique Id
const generateId=()=>{
  return Math.floor(Math.random() * 10000);
}

const handletranactiondeleted =(id)=>{
  transactions.value=transactions.value.filter((transaction)=>transaction.id !==id);
  toast.success('transaction deleted');
}

</script>