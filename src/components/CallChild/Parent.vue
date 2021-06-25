<template>
  parent components
  <div>
    use ref get child data
    <p v-if="RefChild">child value on parent: {{ RefChild.childValue }}</p>
    <button v-on:click="callChild">parent call child method</button>
  </div>
  <Child v-if="a <= 3" ref="RefChild"></Child>
  <template v-if="a <= 3">
    <Child v-for="i in 2" v-bind:key="i" v-bind:ref="el => setItemRef(el, i)"></Child>
  </template>
  

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
    const a = ref(1);
    const RefChild = ref();
    let RefChilds = ref([]);
    const callChild = () => {
      a.value++;
      console.log("single", RefChild.value);
      RefChild.value?.childAdd();

      for (var key in RefChilds.value) {
        RefChilds.value[key].childAdd();
      }
    };
    const setItemRef = (el, key) => {
      // const key = RefChilds.value.length;
      console.log(key, " ", el);
      if (!RefChilds.value.find((cd) => cd === el)) {
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
      RefChilds,
      setItemRef,
      callChild,
      onChildFun,
      callChildCbs,
      cbChildVal,
      setCbChildVal,
      a,
    };
  },
};
</script>
