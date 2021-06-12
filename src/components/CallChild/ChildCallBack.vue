<template>
  <div class="child2">
    <p>Child call back components</p>
    <p>child2 vaue: {{ childValue }}</p>
    <button v-on:click="childAdd">add child value</button>
  </div>
</template>

<script>
import { ref, onUnmounted, watch } from "vue";

export default {
  name: "ChildCallBack",
  props: {
    onParentFun: {
      type: Function,
      required: true,
    },
  },
  setup(props, context) {
    const childValue = ref(0);
    const childAdd = () => {
      childValue.value += 1;
    };

    const cleanUp = props.onParentFun(childAdd);
    onUnmounted(() => cleanUp());

    watch(childValue, (newVal) =>{
        context.emit("setCbChildVal", newVal);
    })
    
    return { childValue, childAdd };
  },
};
</script>
