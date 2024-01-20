<script setup>
import { defineProps } from "vue";
const props = defineProps({
  transaction: { type: Array, required: true },
});
const emit = defineEmits(["transactionDeleted"]);
const deleteTransaction = (id) => {
  emit("transactionDeleted", id);
};
</script>

<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <!-- <li class="minus">
      Cash <span>-$400</span><button class="delete-btn">x</button>
    </li>
    <li class="plus">
      Paycheck <span>$800</span><button class="delete-btn">x</button>
    </li> -->
    <li
      v-for="transaction in transaction"
      v-bind:key="transaction.id"
      :class="transaction.amount > 0 ? 'plus' : 'minus'"
    >
      {{ transaction.text }}
      <span>{{
        transaction.amount > 0
          ? "$" + transaction.amount
          : "-$" + Math.abs(transaction.amount)
      }}</span
      ><button @click="deleteTransaction(transaction.id)" class="delete-btn">
        x
      </button>
    </li>
  </ul>
</template>

<style scoped></style>
