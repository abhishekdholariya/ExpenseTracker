<template>
  <h3>Add new transaction</h3>
  <form id="form">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" v-model="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="bank">Paymet Method </label>
      <select name="bank" id="bank" v-model="bank">
        <option value="" disabled>Select</option>
        <option value="cash">Cash</option>
        <option value="online">Online</option>
      </select>
    </div>
    <div class="form-control">
      <label for="amount"> Amount </label>
      <input type="number" id="amount" v-model="amount" placeholder="Enter amount..." />
    </div>
    <div class="inc-exp-container">
      <button class="btn" @click.prevent="onSubmit(true)">Add Income</button>
      <button class="btn" @click.prevent="onSubmit(false)">Add Expense</button>
    </div>
  </form>
</template>

<script>
import { useToast } from 'vue-toastification';

export default {
  name: 'AddTransaction',
  emits: ['transaction-data'],
  data() {
    return {
      text: '',
      amount: null,
      bank: '',
      toast: useToast(),
    }
  },
  methods: {
    onSubmit(isNegative) {
      if (!this.text || !this.amount || !this.bank) {
        this.toast.error('All fields are required');
      } else {
        const newTransaction = {
          text: this.text,
          amount: parseFloat(this.amount),
          bank: this.bank,
        };
        this.$emit('transaction-data', newTransaction, isNegative);
        this.text = '';
        this.amount = null;
        this.bank = '';
      }
    }
  }
}
</script>