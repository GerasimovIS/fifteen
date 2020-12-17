<template>
  <transition-group
    name="flip"
    class="game-board"
    tag="div"
  >
    <slot />
  </transition-group>
</template>

<style lang="scss">
.flip-list-move {
  transition: transform 1s;
}

.game-board {
  display: grid;

  grid-template-rows: repeat(4, 1fr);
  grid-template-columns: repeat(4, 1fr);

  grid-gap: 8px;

  margin-bottom: 15px;
}

.flip-move {
  transition: transform .2s;
}
</style>
