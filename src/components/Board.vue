<template>
  <div class="board-wrapper">
    <div class="board">
      <BoardItem
        v-for="field in fields"
        :field="field"
        :preview="preview"
        :key="'item-' + field.id"
      ></BoardItem>
    </div>
    <p class="difficult">
      Сложность: <strong>{{ difficult }}</strong>
    </p>
    <button @click="start" class="btn">Старт</button>
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
    let preview = ref(false);
    const number_fields = 25;
    let difficult = ref(3);
    let fields = ref([]);

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

    onBeforeMount(init);

    return {
      number_fields,
      difficult,
      fields,
      init,
      preview,
    };
  },
  methods: {
    start() {
      this.init();
      this.prepareGame();
    },

    prepareGame() {
      this.preview = true;
      for (let i = 0; i < this.difficult; i++) {
        const index = this.rand(0, this.number_fields - 1);
        if (this.fields[index].value !== 1) {
          this.fields[index].value = 1;
        } else {
          i--;
        }
      }

      setTimeout(() => {
        this.preview = false;
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
</style>
