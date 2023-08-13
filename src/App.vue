<template>
  <div id="app">
    <Sidebar ref="sidebarref"></Sidebar>
    <div style="height: 100vh; display: flex; flex-direction: column">
      <span style="font-size: 30px; cursor: pointer" @click="showsidebar"
        >&#9776; Open Sidebar</span
      >
      <Home style="flex: 1"></Home>
    </div>
  </div>
</template>
<script setup>
import { ref } from "vue";
import Sidebar from "./components/SideBar.vue";
import Home from "./views/Home.vue";

const sidebarref = ref(null);
const showsidebar = () => {
  sidebarref.value.opensidebar();
};
</script>

<style lang="scss" scoped></style>
