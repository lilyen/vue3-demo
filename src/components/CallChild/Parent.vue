<template>
  parent components
  <div>
      use ref get child data
    <p v-if="RefChild">child value on parent: {{ RefChild.childValue }}</p>
  <button v-on:click="callChild">parent call child method</button>
  </div>
  <div>
    use call back get child data
    <p>child value on parent: {{ cbChildVal }}</p>
    <button v-on:click="callChildCbs">
    parent use call back call child method
  </button>
  </div>
  

  <Child ref="RefChild"></Child>
  <Child2 v-bind:onParentFun="onChildFun"></Child2>
  <Child2 v-bind:onParentFun="onChildFun" v-on:setCbChildVal="setCbChildVal"></Child2>
</template>

<script>
// @ is an alias to /src
import Child from "@/components/CallChild/Child.vue";
import Child2 from "@/components/CallChild/ChildCallBack.vue";
import { ref } from "vue";

export default {
  name: "Parent",
  components: {
    Child,
    Child2,
  },
  setup() {
    const RefChild = ref();
    const callChild = () => {
      RefChild.value?.childAdd();
    };

    const cbChildVal = ref(0);
    const setCbChildVal = (val)=>{
        cbChildVal.value = val;
    }
    let childCbs = [];
    const onChildFun = (fn) => {
      childCbs.push(fn);
      const cleanUp = () => {
          childCbs = childCbs.filter((item) => item !== fn);
          console.log(`cleanUp call back method ${fn.ref}, childCbs num ${childCbs.length}`);
      };
      return cleanUp;
    };
    const callChildCbs = () => {
      childCbs.forEach((fn) => fn());
    };

    return { RefChild, callChild, onChildFun, callChildCbs, cbChildVal,setCbChildVal  };
  },
};
</script>
