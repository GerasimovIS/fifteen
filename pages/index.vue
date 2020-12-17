<template>
  <div class="container">
    <Game @win="showResult" />
  </div>
</template>

<script>
import Game from '~/components/Game/Game'

export default {
  components: { Game },
  methods: {
    showResult (result) {
      alert(`Счет: ${result.score}, Время: ${result.time}`)
    }
  }
}
</script>

<style lang="scss">
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
</style>
