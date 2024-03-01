<template>
    <div class="inc-exp-container">
    <div>
      <h4>Income</h4>
      <p id="money-plus" class="money plus">$+{{income}}</p>
    </div>
    <div>
      <h4>Expense</h4>
      <p id="money-minus" class="money minus">${{expenses}}</p>
    </div>
  </div>
</template>

<script setup>
import { defineProps } from 'vue';

// Define props for the component using defineProps
const props = defineProps({
  // 'income' prop represents the total income value
  income: {
    type: Number,
    required: true
  },
  // 'expenses' prop represents the total expenses value
  expenses: {
    type: Number,
    required: true
  },
})
</script>