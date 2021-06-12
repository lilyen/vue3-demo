<template>
  <Parent></Parent>
</template>

<script>
// @ is an alias to /src
import Parent from "@/components/CallChild/Parent.vue";

export default {
  name: "CallChild",
  components: {
    Parent,
  },
  setup() {},
};
</script>
