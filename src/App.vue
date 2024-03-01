<script setup>
import Header from './components/Header.vue'
import Balance from './components/Balance.vue'
import IncomeExpenses from './components/IncomeExpenses.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransaction.vue'

import {ref, computed, onMounted} from 'vue'
import {useToast} from 'vue-toastification'

// Create a reactive variable 'transactions' using ref
const transactions = ref([]);

// Hook that runs after the component is mounted to the DOM
onMounted(() => {

  // Retrieve saved transactions from local storage
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
  // Check if there are saved transactions
  if(savedTransactions){
    // Update the 'transactions' variable with the saved transactions
    transactions.value = savedTransactions;
  }
});

// Create a computed property 'total' that calculates the sum of all transaction amounts
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount
  }, 0)
})

// Create a computed property 'income' that calculates the total income
const income = computed(() => {
  return transactions.value
  // Filter transactions to include only positive amounts (income)
  .filter((transaction) => transaction.amount > 0)
  // Reduce filtered transactions to calculate the total income
  .reduce((acc, transaction) => {
    return acc + transaction.amount
  }, 0)
  // Format the result with two decimal places
  .toFixed(2);
})

// Create a computed property 'expenses' that calculates the total expenses
const expenses = computed(() => {
  return transactions.value
  // Filter transactions to include only negative amounts (expenses)
  .filter((transaction) => transaction.amount < 0)
  // Reduce filtered expenses to calculate the total expenses
  .reduce((acc, transaction) => {
    return acc + transaction.amount
  }, 0)
  .toFixed(2);
})

// Create a 'toast' instance to use for displaying toast messages
const toast = useToast();

// Add Transaction
const handleTransactionSubmitted = (transactionData) => {
  // Push a new transaction object to the 'transactions' array
  transactions.value.push({
    id: generateUniqueId(), // Generate a unique ID for the transaction
    text: transactionData.text, // Assign the text from the submitted data
    amount: transactionData.amount // Assign the amount from the submitted data
  });
  // Save the updated transactions to local storage
  saveTransactionsToLocalStorage();
  toast.success("Transaction added")
}

// Generate Unique Id
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
}

// Delete Transaction
const handleTransactionDeleted = (id) => {
  // Update the 'transactions' array by filtering out the transaction with the specified ID
  transactions.value = transactions.value.filter((transaction) =>
    transaction.id !== id
  );
  // Save the updated transactions to local storage
  saveTransactionsToLocalStorage();
  toast.success("Transaction Deleted");
}

// Function to save the 'transactions' array to local storage
const saveTransactionsToLocalStorage = () => {
  // Convert the 'transactions' array to a JSON string and store it in local storage
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
}

// export default{
//   components:{
//     Header,
//     Balance,
//     IncomeExpenses,
//     TransactionList,
//     AddTransaction
//   }
// }
</script>

<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList :transactions="transactions" @transactionDeleted = "handleTransactionDeleted" />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" :total="+total" />
  </div>
</template>