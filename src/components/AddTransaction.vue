<script setup>
  import { ref } from 'vue';
  import { useToast } from 'vue-toastification';

  const text = ref('');
  const amount = ref('');
  const toast = useToast();
  const emit = defineEmits(['submitItem']);

  const onSubmit = () => {
    if (!text.value || !amount.value) {
      toast.error('Both fields must be filled.');
      return;
    }

    const data = {
      text: text.value,
      amount: parseFloat(amount.value).toFixed(2)
    };
    
    emit('submitItem', data);

    text.value = '';
    amount.value = '';
  }
</script>

<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" v-model="text" placeholder="Enter text" />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        <p class="small">(negative amount - expense, positive amount - income)</p>
        </label
      >
      <input type="text" id="amount" v-model="amount" placeholder="Enter amount (negative or positive)" />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>
