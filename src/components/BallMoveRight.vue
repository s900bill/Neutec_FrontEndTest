<template>
  <div class="container">
    <div class="grid">
      <div
        class="cell"
        v-for="cell in cells"
        :key="cell"
        :class="{ active: activeIndices.includes(cell) }"
      >
        <div v-if="ballincorner.includes(cell)" class="ball moveright">0</div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
//建立九宮格
const cells = ref(new Array(9).fill(0).map((_, index) => index));

//閃爍的格子
const activeIndices = ref([2, 4, 8]);

//四顆球的起始位置
const ballincorner = [0, 2, 6, 8];
</script>

<style lang="scss" scoped>
.container {
  position: relative;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  overflow: hidden;
}
.grid {
  display: grid;
  margin: auto 0;

  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
  position: relative;
}

.cell {
  width: 100%;
  height: 100px;
  border: black solid 2px;
  background: radial-gradient(
    circle,
    rgba(113, 81, 95, 1) 81%,
    rgba(0, 0, 0, 1) 100%
  );
  opacity: 1;
  position: relative;
}

.cell.active {
  //利用動畫調整opacity達到閃錯的效果
  animation: flashing 0.5s infinite;
}

@keyframes flashing {
  0%,
  100% {
    opacity: 1;
  }
  50% {
    opacity: 0.6;
  }
}

.ball {
  position: absolute;
  width: 30px;
  height: 30px;
  line-height: 30px;
  text-align: center;
  border-radius: 50%;
  background-color: #a5f12b;
  opacity: 1;
  z-index: 1;
  left: 50px;
  top: 50%;
  transform: translateX(-50%);
  transform: translateY(-50%);
  transition: "transform 0.1s ease";

  //利用動畫讓球往右移動
  animation: moveRight 2s infinite;
}

@keyframes moveRight {
  0% {
    left: 50px;
  }
  100% {
    left: calc(100vw - 100% + 30px);
    //left: calc(300% - 30px);
  }
}
</style>
