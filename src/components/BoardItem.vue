<template>
  <span :class="'item ' + (field.value === 1 ? 'item-active' : '')"></span>
</template>

<script>
export default {
  props: {
    field: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style scoped>
.item {
  position: relative;
  cursor: pointer;
  width: 50px;
  height: 50px;
  background: #ccc;
  display: inline-block;
  margin: 5px;
  transition: 0.4s;
  transform-style: preserve-3d;
}

.item-active {
  background: #42b983cc;
  transform: rotateX(180deg);
}
</style>
