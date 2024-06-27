<template>
  <Header />
  <div class="container">
    <Balance :total="total"/>
    <IncomeExpense :income="income" :expense="expense"/>
    <TransactionList :transactions="filteredTransactions" @delete-transaction="deleteTransaction" @filter-transactions="filterTransactions"/>
    <AddTransaction @transaction-data="transactionData" />
  </div>
</template>
<script>
  import { useToast } from 'vue-toastification';
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpense from './components/IncomeExpense.vue';
  import TransactionList from './components/TransactionList.vue';
  import AddTransaction from './components/AddTransaction.vue';
  
  export default {
    components: {
      Header,
      Balance,
      IncomeExpense,
      TransactionList,
      AddTransaction,
    },
    data() {
      return {
        transactions: [],
        toast: useToast(),
        filter: 'all',
      }
    },
    methods: {
      transactionData(newTransaction, isNegative) {
        this.transactions.push({
          id: this.transactions.length + 1,
          text: newTransaction.text,
          amount: isNegative ? newTransaction.amount : newTransaction.amount * -1,
          bank: newTransaction.bank,
        });
        this.saveTransaction();
        this.toast.success('તમારા હિસાબી ખાતા મા રકમ જમા થઈ ગઈ.');
      },
      deleteTransaction(id) {
        this.transactions = this.transactions.filter((transaction) => transaction.id !== id);
        this.saveTransaction();
        this.toast.success('તમારા હિસાબી ખાતા મા રકમ બાદ થઈ ગઈ.');
      },
      saveTransaction() {
        localStorage.setItem("transactions", JSON.stringify(this.transactions));
      },
      filterTransactions(filter) {
        this.filter = filter;
      }
    },
    computed: {
      total() {
        return this.transactions.reduce((acc, transaction) => {
          return acc + transaction.amount;
        }, 0).toFixed(2);
      },
      income() {
        return this.transactions.filter((transaction) => transaction.amount > 0).reduce((acc, transaction) => {
          return acc + transaction.amount;
        }, 0).toFixed(2);
      },
      expense() {
        return this.transactions.filter((transaction) => transaction.amount < 0).reduce((acc, transaction) => {
          return acc + transaction.amount;
        }, 0).toFixed(2);
      },
      filteredTransactions() {
        if (this.filter === 'all') {
          return this.transactions;
        }
        return this.transactions.filter(transaction => transaction.bank === this.filter);
      }
    },
    mounted() {
      const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
      if (savedTransactions) {
        this.transactions = savedTransactions;
      }
    },
  }
</script>
