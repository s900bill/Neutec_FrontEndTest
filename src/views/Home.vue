<template>
  <div class="container" @mousemove="moveBall">
    <div style="display: flex">
      <button
        v-for="type in ballmovetypelist"
        :key="type.ID"
        @click="currentballmovetype = type.ID"
        style="margin: 3px"
      >
        {{ type.Text }}
      </button>
    </div>
    <BallMoveRight v-if="currentballmovetype === 0" />
    <BallMovetoPoint v-if="currentballmovetype === 1" />
  </div>
</template>

<script setup>
import { ref } from "vue";
import BallMoveRight from "../components/BallMoveRight.vue";
import BallMovetoPoint from "../components/BallMovetoPoint.vue";

const ballmovetypelist = [
  { ID: 0, Text: "球往右移動" },
  { ID: 1, Text: "球往滑鼠移動" },
];
const currentballmovetype = ref(ballmovetypelist[0].ID);
</script>

<style lang="scss" scoped></style>
