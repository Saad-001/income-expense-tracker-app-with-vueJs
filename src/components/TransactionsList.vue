<script setup>
import { defineProps } from "vue";

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(["deleteTransaction"]);
const deleteTransaction = (transactionId) => {
  emit("deleteTransaction", transactionId);
};
</script>

<template>
  <h3>History</h3>
  <h5 v-if="transactions.length === 0" class="text-secondary">
    No Transaction Found
  </h5>
  <ul v-else id="list" class="list">
    <li
      v-for="transaction in transactions"
      :key="transaction.id"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.name }} <span>${{ transaction.amount }}</span
      ><button class="delete-btn" @click="deleteTransaction(transaction.id)">
        x
      </button>
    </li>
  </ul>
</template>
