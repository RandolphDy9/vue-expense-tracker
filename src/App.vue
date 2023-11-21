<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import Footer from "./components/Footer.vue";

import { ref, computed, onMounted } from "vue";
import { useToast } from "vue-toastification";

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});

const toast = useToast();
const transactions = ref([
  { id: 1, text: "Flower", amount: -100 },
  { id: 2, text: "Salary", amount: 100 },
  { id: 3, text: "Book", amount: -250.5 },
  { id: 4, text: "Investment", amount: 600.5 },
]);

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + +transaction.amount;
  }, 0).toFixed(2);
});

const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => acc + +transaction.amount, 0)
    .toFixed(2);
});

const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => acc + +transaction.amount, 0)
    .toFixed(2);
});

const handleSubmittedItem = (data) => {
  transactions.value.push({
    id: generateId(),
    text: data.text,
    amount: data.amount,
  });

  toast.success("Successfully added.");
  saveTransactionsLocal();
};

const generateId = () => {
  return Math.floor(Math.random() * 1000000);
};

const handleDelete = (id) => {
  transactions.value = transactions.value.filter((transaction) => transaction.id !== id);
  toast.success('Item has been deleted.');
  saveTransactionsLocal();
}

const saveTransactionsLocal = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
}
</script>

<template>
  <div class="app-container">
    <div class="row">
      <div>
        <Header />
        <Balance :total="+total" />
        <IncomeExpenses :income="+income" :expenses="+expenses" />
      </div>
      <div>
        <TransactionList :transactions="transactions" @deleteItem="handleDelete" />
      </div>
      <div>
        <AddTransaction @submitItem="handleSubmittedItem" />
      </div>
    </div>
    <Footer />
  </div>
</template>
