<template>
  <div class="board-wrapper">
    <div class="board">
      <BoardItem
        v-for="field in fields"
        :field="field"
        :gameStatus="gameStatus"
        :key="'item-' + field.id"
        @selectField="selectField($event)"
      ></BoardItem>
    </div>
    <p class="difficult">
      Сложность: <strong>{{ difficult }}</strong>
    </p>
    <button @click="start" :disabled="isDisabled" class="btn">Старт</button>
  </div>
</template>

<script>
import { ref, onBeforeMount } from "vue";
import BoardItem from "./BoardItem.vue";
export default {
  name: "board-app",
  components: {
    BoardItem,
  },
  setup() {
    const number_fields = 25;
    let isDisabled = ref(false);
    let difficult = ref(3);
    let fields = ref([]);
    let gameStatus = ref(0);
    let game_speed = 2000;

    const init = () => {
      fields.value = [];
      for (let i = 0; i < number_fields; i++) {
        fields.value.push({
          id: i,
          clicked: false, // кликнул или нет ещё
          value: 0, // пустая клетка, если 1 то значит её нужно найти.
        });
      }
    };

    const start = () => {
      init();
      prepareGame();
    };

    const prepareGame = () => {
      isDisabled.value = true;
      gameStatus.value = 1;
      for (let i = 0; i < difficult.value; i++) {
        const index = rand(0, number_fields - 1);
        if (fields.value[index].value !== 1) {
          fields.value[index].value = 1;
        } else {
          i--;
        }
      }

      setTimeout(() => {
        gameStatus.value = 2;
        isDisabled.value = false;
      }, game_speed);
    };

    const rand = (min, max) => {
      return Math.floor(Math.random() * (max - min)) + min;
    };

    const selectField = (id) => {
      const index = fields.value.findIndex((field) => {
        return field.id === id;
      });

      if (index > -1 && !fields.value[index].clicked) {
        fields.value[index].clicked = true;
        checkGame();
      }
    };

    const checkGame = () => {
      const errorIndex = fields.value.findIndex((field) => {
        return field.clicked && field.value === 0;
      });

      if (errorIndex > -1) {
        setGameOver();
        return;
      }

      const notFoundItemIndex = fields.value.findIndex((field) => {
        return !field.clicked && field.value === 1;
      });

      if (notFoundItemIndex === -1) {
        setWin();
      }
    };

    const setGameOver = () => {
      gameStatus.value = 4;
      setTimeout(() => {
        difficult.value = 3;
      }, game_speed);
    };

    const setWin = () => {
      gameStatus.value = 3;
      setTimeout(() => {
        difficult.value += 1;
        if (difficult.value > 10) {
          difficult.value = 10;
        }
        start();
      }, game_speed);
    };

    onBeforeMount(init);

    return {
      number_fields,
      difficult,
      fields,
      isDisabled,
      gameStatus,
      game_speed,
      init,
      start,
      selectField,
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

.btn:disabled {
  opacity: 0.5;
}
</style>
