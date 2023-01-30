<template>
  <div class="board-wrapper">
    <div class="board">
      <BoardItem v-for="i in 25" :key="'item-' + i"></BoardItem>
    </div>
    <p class="difficult">
      Сложность: <strong>{{ difficult }}</strong>
    </p>
    <button class="btn">Старт</button>
  </div>
</template>

<script>
import { ref } from "vue";
import BoardItem from "./BoardItem.vue";
export default {
  name: "board-app",
  components: {
    BoardItem,
  },
  setup() {
    const difficult = ref(3);

    return {
      difficult,
    };
  },
};
</script>

<style scoped>
.board-wrapper {
  margin-bottom: 100px;
}
.board {
  width: 300px;
  background: #eee;
  margin: 0 auto;
}

.btn {
  background: #42b983cc;
  color: white;
  border: none;
  border-radius: 2px;
  padding: 10px;
  margin: 10px 0;
  cursor: pointer;
  outline: none;
}

.btn:hover {
  background: #42b983;
}
</style>
