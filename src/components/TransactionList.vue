<template>
    <h3>History</h3>
      <ul id="list" class="list">
        <li 
          v-for="transaction in transactions"
          :key="transaction.id" :class="transaction.amount < 0 ? 'minus' : 'plus'">
          {{transaction.text}} <span>${{transaction.amount}}</span>
          <button @click="deleteTransaction(transaction.id)" class="delete-btn">x</button>
        </li>
        <!-- <li class="minus">
          Cash <span>-$400</span><button class="delete-btn">x</button>
        </li>
        <li class="plus">
          Paycheck <span>$800</span><button class="delete-btn">x</button>
        </li> -->
      </ul>
</template>

<script setup>
import { defineProps } from 'vue';

// Define props for the component using defineProps
const props = defineProps({
  // 'transactions' prop represents an array of transactions
  transactions: {
    type: Array,
    required: true
  }
});
// Define emits to handle the 'transactionDeleted' event
const emit = defineEmits(['transactionDeleted']);
// Function to handle the deletion of a transaction by ID
const deleteTransaction = (id) => {
  // Emit the 'transactionDeleted' event with the transaction ID
  emit('transactionDeleted', id);
}

// Using Composition API
// const transactions = [
//         { id: 1, text: 'Flower', amount: -19.99},
//         { id: 2, text: 'Salary', amount: 299.97},
//         { id: 3, text: 'Book', amount: -10},
//         { id: 4, text: 'Camera', amount: 150}
//       ]

// Using Options API
// export default{
//   data(){
//     return{
//       transactions: [
//         { id: 1, text: 'Flower', amount: -19.99},
//         { id: 2, text: 'Salary', amount: 299.97},
//         { id: 3, text: 'Book', amount: -10},
//         { id: 4, text: 'Camera', amount: 150}
//       ]
//     }
//   }
// }
</script>