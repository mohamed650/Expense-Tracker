<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" v-model="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount">Amount</label><br>
      <label id="inclabel">
        <input type="radio" name="incexpbtn" v-model="incexpVal" value="income" checked>
        Income</label>
      <label id="explabel">
        <input type="radio" name="incexpbtn" v-model="incexpVal" value="expense">
        Expense</label>
      <input type="text" id="amount" v-model="amount" @input="filterNonNumeric" placeholder="Enter amount..." />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import { ref, defineProps, toRefs } from 'vue'
import { useToast } from 'vue-toastification'

// Create reactive variables for text and amount using ref
const text = ref('');
const amount = ref('');
// Define emits to handle the 'transactionSubmitted' event
const emit = defineEmits(['transactionSubmitted']);
// Use the toast function from the toast library
const toast = useToast();
// Function to filter non-numeric characters from the 'amount' input
const filterNonNumeric = () => {
  amount.value = amount.value.replace(/[^0-9.]/g, '');
  // Ensure only one decimal point is allowed
  const decimalCount = (amount.value.match(/\./g) || []).length;
  if (decimalCount > 1) {
    amount.value = amount.value.replace(/\.+$/, '');
  }

  // Allow only two digits after the decimal point
  const parts = amount.value.split('.');
  if (parts.length > 1 && parts[1].length > 2) {
    amount.value = parts[0] + '.' + parts[1].slice(0, 2);
  }
}

// Define props for the component using defineProps
const props = defineProps({
  // 'total' prop represents the total value
  total: {
    type: Number,
    required: true
  }
});

const incexpVal = ref('')
// Function to handle form submission
const onSubmit = () => {
  if (!text.value || !amount.value) {
    toast.error("Both fields must be filled");
    return;
  } else if (text.value.trim() === "" || amount.value.trim() === "") {
    toast.error("Whitespaces are not allowed");
    return;
  }

  const { total } = toRefs(props);

  if (total.value === 0 && incexpVal.value === 'expense') {
    toast.error("Balance is Nill cannot add expenses");
  } else if (total.value < amount.value && incexpVal.value === 'expense') {
    toast.error("Expense cannot be greater than Balance");
  } else {
    if (incexpVal.value === 'expense') {
      amount.value = '-' + amount.value
    }
    const transactionData = {
      text: text.value,
      amount: parseFloat(amount.value)
    };
    // Emit 'transactionSubmitted' event with the transaction data
    emit('transactionSubmitted', transactionData);
    // Reset text and amount values after submission
    text.value = ''
    amount.value = ''
  }
}
</script>

<style scoped>
#explabel {
  margin-left: 10px;
}
</style>