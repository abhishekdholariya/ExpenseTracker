<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <div v-if="transactions == ''">
      <li>No List</li>
    </div>
    <div v-else>
      <li v-for="transaction in transactions" :key="transaction.id" :class="transaction.amount < 0 ? 'minus' : 'plus'">
        {{ transaction.text }} <span>${{ transaction.amount }}</span>
        <button @click="deleteTrancation(transaction.id)" class="delete-btn">X</button>
      </li>
    </div>
  </ul>
</template>

<script>
export default {
  name: "TransactionList",
  props: {
    transactions: Array,
  },
  emits: ["delete-transaction"],
  methods: {
    deleteTrancation(id) {
      this.$emit("delete-transaction", id);
    },
  },
};
</script>
