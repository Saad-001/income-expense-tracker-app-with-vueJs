<script setup>
import { ref } from "vue";
import { useToast } from "vue-toastification";

const toast = useToast();
const name = ref("");
const amount = ref("");
const emit = defineEmits(["addTransaction"]);

const onSubmit = () => {
  if (!name.value || !amount.value) {
    toast.error("Both fields must be filled!");
    return;
  }

  const transactionData = {
    name: name.value,
    amount: parseFloat(amount.value),
  };

  emit("addTransaction", transactionData);

  name.value = "";
  amount.value = "";
};
</script>

<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" v-model="name" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        type="number"
        id="amount"
        v-model="amount"
        placeholder="Enter amount..."
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>
