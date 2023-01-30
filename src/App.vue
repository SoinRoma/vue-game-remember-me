<template>
  <div class="app">
    <img class="logo" alt="Vue logo" src="./assets/logo.png" />
    <h1 class="h1">Запомни меня!</h1>
    <p>Запомни и открой все зелёные клетки</p>
    <Board></Board>
  </div>
</template>

<script>
import Board from "@/components/Board.vue";

export default {
  name: "App",
  components: { Board },
};
</script>

<style scoped>
.app {
  text-align: center;
  margin-top: 60px;
}
.logo {
  width: 100px;
}
</style>
