<script setup>
import { ref, computed, onMounted } from "vue";
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpense from "./components/IncomeExpense.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { useToast } from "vue-toastification";
const toast = useToast();
const transaction = ref([]);
onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem("transaction"));
  if (savedTransactions) {
    transaction.value = savedTransactions;
  }
});
const total = computed(() => {
  return transaction.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});
const income = computed(() => {
  return transaction.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});
const expense = computed(() => {
  return transaction.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});
const handleTransactionSubmitted = (transactionData) => {
  transaction.value.push({
    id: generateUniqueid(),
    text: transactionData.text,
    amount: transactionData.amount,
  });
  savedTransactionsToLocalStorage();
  toast.success("Transaction added");
};
const generateUniqueid = () => {
  return Math.floor(Math.random() * 1000000);
};
const handleTransactiondeleted = (id) => {
  transaction.value = transaction.value.filter(
    (transaction) => transaction.id !== id
  );
  savedTransactionsToLocalStorage();
  toast.success("Transaction deleted");
};
const savedTransactionsToLocalStorage = () => {
  localStorage.setItem("transaction", JSON.stringify(transaction.value));
};
</script>

<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpense :expense="+expense" :income="+income" />
    <TransactionList
      @transactionDeleted="handleTransactiondeleted"
      :transaction="transaction"
    />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>

<style scoped></style>
