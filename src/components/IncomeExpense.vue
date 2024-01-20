<script setup>
import { defineProps } from "vue";
const props = defineProps({
  expense: { type: Number, required: true },
  income: { type: Number, required: true },
});
</script>

<template>
  <div class="inc-exp-container">
    <div>
      <h4>Income</h4>
      <p id="money-plus" class="money plus">{{ "+$" + Math.abs(income) }}</p>
    </div>
    <div>
      <h4>Expense</h4>
      <p id="money-minus" class="money minus">{{ "-$" + Math.abs(expense) }}</p>
    </div>
  </div>
</template>

<style scoped></style>
