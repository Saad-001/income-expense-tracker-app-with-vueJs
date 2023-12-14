<script setup>
import { computed, onMounted, ref } from "vue";
import { useToast } from "vue-toastification";
import AddTransaction from "./components/AddTransaction.vue";
import Balance from "./components/Balance.vue";
import Header from "./components/Header.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionsList from "./components/TransactionsList.vue";

const transactions = ref([]);

onMounted(() => {
  const savedTransactions = localStorage.getItem("transactions");
  if (savedTransactions) {
    transactions.value = JSON.parse(savedTransactions);
  }
});

// total amount calculation
const total = computed(() =>
  transactions.value.reduce((acc, transaction) => acc + transaction.amount, 0)
);

// income calculation
const income = computed(() =>
  transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2)
);

// expenses calculation
const expenses = computed(() =>
  transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2)
);
const toast = useToast();

const handleAddTransaction = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    name: transactionData.name,
    amount: transactionData.amount,
  });

  saveTransactionToLocalStorage();
  toast.success("Transaction added successfully!");
};

const generateUniqueId = () => {
  return Math.floor(Math.random() * 100000000);
};

const handleDeleteTransaction = (transactionId) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== transactionId
  );

  saveTransactionToLocalStorage();
  toast.success("Transaction deleted successfully!");
};

const saveTransactionToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script>

<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionsList
      :transactions="transactions"
      @deleteTransaction="handleDeleteTransaction"
    />
    <AddTransaction @addTransaction="handleAddTransaction" />
  </div>
</template>
