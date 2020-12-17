<template>
  <div
    class="game-cell"
    @click="$emit('cell-click', $event, item)"
  >
    <div>{{ `${item.value || ''}` }}</div>
  </div>
</template>

<script>
export default {
  props: {
    item: {
      type: Object,
      required: true
    }
  }
}
</script>

<style lang="scss">
.game-cell {
  display: flex;
  align-items: center;
  justify-content: center;

  height: 94px;

  border: 1px solid #ccc;
  border-radius: 4px;

  font-family: sans-serif;
  font-weight: bold;
  font-size: 19px;
  color: #35495e;

  background-color: #FFF;

  user-select: none;
  cursor: pointer;

  transition: all .2s ease;

  &:hover {
    transform: scale(1.05);
  }
}
</style>
