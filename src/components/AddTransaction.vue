<template>
    <h3>Add new transaction</h3>
    <div></div>
    <form id="form">
      <div class="form-control">
        <label for="text">Text</label>
        <input type="text" id="text" v-model="text" placeholder="Enter text..." />
      </div>
      <div class="form-control">
        <label for="amount">
          Amount <br />
          (negative - expense, positive - income)
        </label>
        <input type="number" min="0" id="amount" v-model="amount" placeholder="Enter amount..." />
      </div>
      <div class="inc-exp-container">
        <button class="btn" @click.prevent="onSubmit(true)">Add Income</button>
        <button class="btn" @click.prevent="onSubmit(false)">Add Expense</button>
      </div>
    </form>
  </template>
  
  <script>
  import {useToast} from 'vue-toastification';
  
  export default {
    name: 'AddTransaction',
    emits: ['transaction-data'],
    data() {
      return {
        text: '',
        amount: null,
        toast: useToast()
      }
    },
    methods: {
      onSubmit(isNegtive) {
        if (!this.text || !this.amount) {
          this.toast.error('Both fields are required');
        } else {
          const newTransaction = {
            text: this.text,
            amount: parseFloat(this.amount)
          };
          this.$emit('transaction-data', newTransaction, isNegtive);
          this.text = '';
          this.amount = null;
        }
      }
    }
  }
  </script>
  