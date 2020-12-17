<template>
  <div class="game-stats">
    <div class="game-stats__item">
      Счет: <br>{{ score }}
    </div>
    <div class="game-stats__item">
      Время: <br>{{ time }}
    </div>
  </div>
</template>

<script>
export default {
  props: {
    score: {
      type: Number,
      default: 0
    },
    time: {
      type: String,
      default: '00:00'
    }
  }
}
</script>

<style lang="scss">
.game-stats {
  display: flex;
  margin-bottom: 15px;

  &__item {
    flex-grow: 1;
    flex-basis: 50%;
    margin: 5px;
    padding: 5px;
    border: 1px solid #FFF;
    color: #FFF;
    border-radius: 4px;
    font-weight: bold;

    &:first-child {
      margin-left: 0;
    }

    &:last-child {
      margin-right: 0;
    }
  }
}
</style>
