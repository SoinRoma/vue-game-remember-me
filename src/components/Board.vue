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

    const selectField = (id) => {
      const index = fields.value.findIndex((field) => {
        return field.id === id;
      });

      if (index > -1) {
        fields.value[index].clicked = true;
      }
    };

    onBeforeMount(init);

    return {
      number_fields,
      difficult,
      fields,
      init,
      selectField,
      isDisabled,
      gameStatus,
    };
  },
  methods: {
    start() {
      this.init();
      this.prepareGame();
    },

    prepareGame() {
      this.isDisabled = true;
      this.gameStatus = 1;
      for (let i = 0; i < this.difficult; i++) {
        const index = this.rand(0, this.number_fields - 1);
        if (this.fields[index].value !== 1) {
          this.fields[index].value = 1;
        } else {
          i--;
        }
      }

      setTimeout(() => {
        this.gameStatus = 2;
        this.isDisabled = false;
      }, 2000);
    },

    rand(min, max) {
      return Math.floor(Math.random() * (max - min)) + min;
    },
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
