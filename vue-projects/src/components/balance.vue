<template>
    <h4>Your Balance</h4>
    <h1 id="balance">${{ formatTotal }}</h1>

</template>

<script setup>
import { computed } from 'vue';

const props = defineProps({
  total: {
    type: Number,
    required: true
  }
});


const formatTotal = computed(() => {
  return props.total.toFixed(2);
});
</script>