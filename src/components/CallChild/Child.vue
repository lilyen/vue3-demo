<template>
  <div class="child">
    <p>Child components</p>
    <p>child vaue: {{ childValue }}</p>
    <button v-on:click="childAdd">add child value</button>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  name: "Child",
  setup() {
    const childValue = ref(0);
    const childAdd = () => {
      childValue.value += 1;
    };
    return { childValue, childAdd };
  },
};
</script>
