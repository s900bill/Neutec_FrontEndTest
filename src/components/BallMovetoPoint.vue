<template>
  <div class="container" @mousemove="mousemove">
    <div>
      <label>球的數量:</label>
      <input v-model="ballcount" type="number" />
    </div>
    <div>移動到</div>
    <label>
      <input type="radio" :value="'mouse'" v-model="movetype" />
      <span>滑鼠座標</span>
    </label>
    <label>
      <input type="radio" :value="'coordinates'" v-model="movetype" />
      <span>
        <span>指定</span>
        <label>x:</label>
        <input v-model="ballposition.x" type="number" />
        <label>y:</label>
        <input v-model="ballposition.y" type="number" />
      </span>
    </label>
    <div style="margin: 3px">
      <button @click="moveBalls" style="margin-right: 5px">移動球</button>
      <button @click="stopMoving">停止移動</button>
    </div>

    <div class="grid">
      <div
        class="cell"
        v-for="cell in cells"
        :key="cell"
        :class="{ active: activeIndices.includes(cell) }"
      ></div>
    </div>
    <div v-for="ball in balls" :key="ball.ID">
      <div class="ball" :style="ball.style">{{ ball.id }}</div>
    </div>
  </div>
</template>

<script setup>
import { ref, watch, onMounted } from "vue";
const movetype = ref("mouse");
const cells = ref(new Array(9).fill(0).map((_, index) => index));
const activeIndices = ref([2, 4, 8]);

const ballcount = ref(4);
const balls = ref([]);
watch(
  () => ballcount.value,
  (count) => {
    const newballs = [];
    for (let i = 0; i < count; i++) {
      newballs.push({
        id: i + 1,
        style: {
          left: Math.random() * 450 + "px",
          top: Math.random() * 450 + "px",
        },
      });
    }
    balls.value = newballs;
  },
  { immediate: true }
);

const ballposition = ref({
  x: 0,
  y: 0,
});
const mousemove = (event) => {
  if (movetype.value !== "mouse") return;
  ballposition.value.x = event.clientX;
  ballposition.value.y = event.clientY;
};

let numSteps = 10; // 移動的步數
let moving = false; // 球是否要移動

const moveStep = () => {
  if (numSteps > 0) {
    if (!moving) return;
    balls.value.forEach((ball) => {
      const currentX = parseFloat(ball.style.left);
      const currentY = parseFloat(ball.style.top);
      const deltaX = (ballposition.value.x - currentX - 8) / numSteps;
      const deltaY = (ballposition.value.y - currentY - 77) / numSteps;

      ball.style.left = currentX + deltaX + "px";
      ball.style.top = currentY + deltaY + "px";
    });

    numSteps--;
    requestAnimationFrame(moveStep);
  } else {
    numSteps = 100; // 重置步数
    balls.value.forEach((ball) => {
      ball.style.left = Math.random() * 450 + "px";
      ball.style.top = Math.random() * 450 + "px";
    });
    moving = false;
    moveBalls(); //繼續移動
  }
};

//開始移動球
const moveBalls = () => {
  if (!moving) {
    moving = true;
    moveStep();
  }
};

//停止移動球
const stopMoving = () => {
  moving = false;
};

onMounted(() => {
  moveBalls();
});
</script>

<style lang="scss" scoped>
input {
  width: 50px;
}
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
}

.cell.active {
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
}
</style>
