<template>
  <div class="section">
    <div class="select">
      <select v-model="dropdownvalue" @change="dropdown_change">
        <option disabled :value="[]">請選擇</option>
        <option
          v-for="item in dropdownlist"
          v-bind:key="item.key"
          :value="item.treeid"
        >
          {{ item.text }}
        </option>
      </select>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from "vue";
const prop = defineProps({
  data: { type: Array, required: true, default: [] },
  currentselect: { type: Array, required: true, default: [] },
});
const emit = defineEmits(["select"]);
const dropdownlist = computed(() => {
  let list = [];
  const getalldata = (datas) => {
    for (const d of datas) {
      list.push({ key: d.key, text: d.text, treeid: d.treeid });
      if (d.children) {
        getalldata(d.children);
      }
    }
  };
  getalldata(prop.data);
  return list;
});
const dropdownvalue = ref([]);
watch(
  () => prop.currentselect,
  (newvalue) => {
    if (newvalue) {
      dropdownvalue.value = newvalue;
    }
  }
);
const dropdown_change = () => {
  emit("select", dropdownvalue.value);
};
</script>

<style lang="scss" scoped>
ul,
li {
  color: white;
  list-style-type: none;
  cursor: pointer;
  white-space: nowrap;
}
ul {
  list-style-type: none;
  padding-left: 1rem;

  font-size: 1.5rem;
}
li {
  padding-top: 1rem;
  &:hover {
    color: #223ef4;
  }
  &.active {
    color: #c9c956;
    background-color: #808080;
  }
}
</style>
