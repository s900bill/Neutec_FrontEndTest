<template>
  <ul
    v-for="item in data"
    :key="item.key"
    :style="{
      backgroundColor: prop.currentselect.includes(item.key) ? '#808080' : '',
    }"
  >
    <li
      @click="ItemClick(item.treeid)"
      :class="{ active: prop.currentselect.includes(item.key) }"
    >
      {{ item.text }}
    </li>
    <Treeview
      v-if="
        item.children &&
        item.children.length > 0 &&
        prop.currentselect.includes(item.key)
      "
      :data="item.children"
      :currentselect="prop.currentselect"
      @select="ItemClick"
    >
    </Treeview>
  </ul>
</template>

<script setup>
const prop = defineProps({
  data: { type: Array, required: true, default: [] },
  currentselect: { type: Array, required: true, default: [] },
});
const emit = defineEmits(["select"]);
const ItemClick = (value) => {
  emit("select", value);
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
