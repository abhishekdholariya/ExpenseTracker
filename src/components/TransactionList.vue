  <template>
    <div class="filter-buttons">
      <h3>History</h3>
      <button @click="filterTransactions('all')">All</button>
      <button @click="filterTransactions('cash')">Cash</button>
      <button @click="filterTransactions('online')">Online</button>
    </div>
    <ul id="list" class="list">
      <div v-if="transactions == ''">
        <li>No List</li>
      </div>
      <div v-else>
        <li v-for="transaction in transactions" :key="transaction.id" :class="transaction.amount < 0 ? 'minus' : 'plus'">
          {{ transaction.text }} <span>{{ transaction.bank }}</span> <span>${{ transaction.amount }} </span>
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
    emits: ["delete-transaction", "filter-transitions"],
    methods: {
      deleteTrancation(id) {
        this.$emit("delete-transaction", id);
      },
      filterTransactions(filter) {
        this.$emit("filter-transactions", filter);
      }
    },
  };
  </script>
  