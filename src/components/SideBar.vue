<template>
  <div v-if="isOpen" class="sidebarwapper" @click="closesidebar()"></div>

  <div
    id="mySidenav"
    class="sidebar"
    :style="{ width: isOpen ? '250px' : '0px' }"
  >
    <i class="closebtn" @click="closesidebar()">&times;</i>
    <Dropdown
      :data="drinklist_arrang"
      :currentselect="currentselect"
      @select="ItemClick"
    ></Dropdown>
    <Treeview
      :data="drinklist_arrang"
      :currentselect="currentselect"
      @select="ItemClick"
    ></Treeview>
  </div>
</template>
<script setup>
import { ref, onMounted } from "vue";
import Treeview from "./Treeview.vue";
import Dropdown from "./Dropdown.vue";
const prop = defineProps({ showsidebar: Boolean });
const emit = defineEmits(["close"]);
const isOpen = ref(false);
const opensidebar = () => {
  isOpen.value = true;
};
const closesidebar = () => {
  isOpen.value = false;
};
defineExpose({
  opensidebar,
});

//從API拿來的原始Data
const drinklist = [
  {
    key: "64f",
    text: "好喝黑糖",
    children: [
      {
        key: "445",
        text: "黑糖鮮奶",
        children: [
          {
            key: "37a",
            text: "黑糖珍珠鮮奶",
          },
          {
            key: "feb",
            text: "黑糖芋圓鮮奶",
          },
          {
            key: "59c",
            text: "黑糖蒟蒻鮮奶",
          },
        ],
      },
      {
        key: "29e",
        text: "黑糖冬瓜",
        children: [
          {
            key: "ac3",
            text: "黑糖冬瓜牛奶",
          },
          {
            key: "ca0",
            text: "黑糖冬瓜珍珠",
          },
        ],
      },
    ],
  },
  {
    key: "6c3",
    text: "茶",
    children: [
      {
        key: "5dc",
        text: "烏龍綠",
      },
      {
        key: "b5f",
        text: "綠茶",
      },
      {
        key: "b09",
        text: "紅茶",
      },
      {
        key: "887",
        text: "青茶",
      },
    ],
  },
  {
    key: "c81",
    text: "咖啡",
    children: [
      {
        key: "e02",
        text: "黑咖啡",
        children: [
          {
            key: "d20",
            text: "濃縮咖啡",
          },
          {
            key: "1f8",
            text: "美式咖啡",
          },
        ],
      },
      {
        key: "d7a",
        text: "牛奶咖啡",
        children: [
          {
            key: "c09",
            text: "拿鐵",
            children: [
              {
                key: "db2",
                text: "黑糖拿鐵",
              },
              {
                key: "9f6",
                text: "榛果拿鐵",
              },
              {
                key: "b61",
                text: "香草拿鐵",
              },
            ],
          },
          {
            key: "9ac",
            text: "卡布奇諾",
          },
          {
            key: "ce8",
            text: "摩卡",
          },
        ],
      },
    ],
  },
];

//處理Data，幫每一層加上treeid
const drinklist_arrang = drinklist.map((x) => {
  let newx = { ...x, treeid: [x.key] };

  const setparent = (data, parent) => {
    return data.map((c) => {
      let newc = { ...c, treeid: [...parent, c.key] };
      if (c.children) {
        newc.children = setparent(c.children, newc.treeid);
      }
      return newc;
    });
  };
  if (x.children) {
    newx.children = setparent(x.children, newx.treeid);
  }
  return newx;
});
const currentselect = ref([]);
onMounted(() => {
  //取得localStorage資料記錄在currentselect內
  let storedata = localStorage.getItem("selectitem");
  if (storedata) {
    currentselect.value = JSON.parse(storedata);
  }
});
const ItemClick = (value) => {
  //點擊選項時，將treeid記錄到localstorage
  currentselect.value = value;
  localStorage.setItem("selectitem", JSON.stringify(currentselect.value));
};
</script>

<style lang="scss" scoped>
.sidebarwapper {
  height: 100%;
  width: 100%;
  position: fixed;
  top: 0;
  right: 0;
  z-index: 1;
}
.sidebar {
  height: 100%;
  width: 0px;
  opacity: 0.9;
  position: fixed;
  z-index: 2;
  top: 0;
  right: 0;
  background-color: #111;
  overflow: auto;
  transition: 0.5s;
  padding-top: 60px;
  color: #818181;
}

.sidebar .closebtn {
  color: #f1f1f1;
  cursor: pointer;
  position: absolute;
  top: 0;
  right: 25px;
  font-size: 36px;
  transition: 0.3s;
  &:hover {
    transform: rotate(180deg);
  }
}
</style>
