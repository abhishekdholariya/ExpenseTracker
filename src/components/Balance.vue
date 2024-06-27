<template>
    <h4>Your Balance</h4>
    <h1 :class="balanceClass" id="balance">${{ total }}</h1>
  </template>
  
  <script>
    export default {
      name: 'Balance',
      props: ['total'],
      computed: {
        balanceClass() {
          return {
            'positive-balance': this.total >= 0,
            'negative-balance': this.total < 0
          };
        }
      }
    }
</script>
  