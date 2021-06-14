<template>
  parent components
  <div>
    use ref get child data
    <p v-if="RefChild">child value on parent: {{ RefChild.childValue }}</p>
    <button v-on:click="callChild">parent call child method</button>
  </div>
  <div>
    use call back get child data
    <p>
      child value on parent:
      <template v-for="v in cbChildVal"> {{ v }}, </template>
    </p>
    <button v-on:click="callChildCbs">
      parent use call back call child method
    </button>
  </div>

  <Child ref="RefChild"></Child>
  <Child v-for="i in 2" v-bind:key="i" v-bind:ref="setItemRef"></Child>
  <Child2
    v-for="i in 2"
    v-bind:key="i"
    v-bind:keyVal="i - 1"
    v-bind:onParentFun="onChildFun"
    v-on:setCbChildVal="setCbChildVal"
  ></Child2>
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
    let RefChilds = ref([]);
    const callChild = () => {
      console.log('single', RefChild.value);
      RefChild.value?.childAdd();

      for(var key in RefChilds.value){
        RefChilds.value[key].childAdd();
      }      
    };
    const setItemRef = (el) => {
      const key = RefChilds.value.length;
      console.log(key, ' ', el)
      if(!RefChilds.value.find(cd=> cd === el)){
        RefChilds.value[key] = el;
      }
    };

    const cbChildVal = ref([]);
    const setCbChildVal = (index, val) => {
      cbChildVal.value[index] = val;
    };
    let childCbs = [];
    const onChildFun = (fn) => {
      childCbs.push(fn);
      const cleanUp = () => {
        childCbs = childCbs.filter((item) => item !== fn);
        console.log(
          `cleanUp call back method ${fn.ref}, childCbs num ${childCbs.length}`
        );
      };
      return cleanUp;
    };
    const callChildCbs = () => {
      childCbs.forEach((fn) => fn());
    };

    return {
      RefChild,
      setItemRef,
      callChild,
      onChildFun,
      callChildCbs,
      cbChildVal,
      setCbChildVal,
    };
  },
};
</script>
