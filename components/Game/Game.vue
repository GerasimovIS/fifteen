<template>
  <div class="game-container">
    <client-only>
      <Stats :score="score" :time="displayTime" />
      <Board>
        <Cell
          v-for="item in items"
          :key="item.id"
          :item="item"
          @cell-click="step"
        />
      </Board>
      <button class="restart-btn" @click="stopGame">
        Начать заново
      </button>
    </client-only>
  </div>
</template>

<script>
import shuffle from 'lodash.shuffle'
import uid from 'lodash.uniqueid'

import Stats from './Stats'
import Board from './Board'
import Cell from './Cell'

export default {
  components: {
    Stats,
    Board,
    Cell
  },

  data () {
    return {
      time: 0,
      timeInterval: null,
      items: [],
      initialItems: [],
      score: 0,
      gameStarted: false,
      directions: {
        UP: 4,
        RIGHT: 1,
        DOWN: -4,
        LEFT: -1
      }
    }
  },

  computed: {
    emptyCellIndex () {
      return this.items.findIndex(i => i.value === 0)
    },
    isSolved () {
      const initial = this.initialItems.map(i => i.value).join('')
      const current = this.items.map(i => i.value).join('')

      return initial === current
    },
    displayTime () {
      const minutes = `${Math.floor((this.time / 60) % 60)}`
      const seconds = `${this.time % 60}`

      return `${minutes.padStart(2, '0')}:${seconds.padStart(2, '0')}`
    }
  },

  created () {
    this.items = this.generateItems(4)
  },

  methods: {
    step (event, item) {
      if (!this.gameStarted) {
        this.startGame()
      }

      const itemIndex = this.items.findIndex(i => i.id === item.id)

      const move = this.emptyCellIndex - itemIndex

      if (move === this.directions.LEFT || move === this.directions.RIGHT) {
        if (Math.floor(this.emptyCellIndex / 4) !== Math.floor(itemIndex / 4)) {
          return
        }
      }

      if (Object.values(this.directions).some(d => d === move)) {
        this.swap(itemIndex, this.emptyCellIndex)
        this.score++
      }

      if (this.isSolved) {
        this.$emit('win', {
          score: this.score,
          time: this.displayTime
        })

        this.stopGame()
      }
    },
    startGame () {
      this.gameStarted = true
      this.timeInterval = setInterval(() => { this.time++ }, 1000)
    },
    stopGame () {
      this.gameStarted = false
      this.score = 0
      this.time = 0
      clearInterval(this.timeInterval)

      this.items = [...shuffle(this.items.filter(i => i.value !== 0)), this.items[this.emptyCellIndex]]
    },
    swap (firstIndex, secondIndex) {
      const arr = this.items.concat();
      [arr[secondIndex], arr[firstIndex]] = [arr[firstIndex], arr[secondIndex]]
      this.items = arr
    },
    generateItems (size) {
      const arr = Array.from(
        { length: Math.pow(size, 2) - 1 },
        (_, index) => ({ id: uid(), value: index + 1 })
      )
      const lastItem = { id: uid(), value: 0 }

      this.initialItems = [
        ...arr,
        lastItem
      ]

      return [...arr, lastItem]
    }
  }
}
</script>

<style lang="scss">
.game-container {
  display: flex;
  flex-direction: column;
  max-width: 400px;
  width: 100%;

  background-color: cornflowerblue;
  padding: 15px;
  border-radius: 4px;
}

.restart-btn {
  width: 100%;
  border: 1px solid #FFF;
  background-color: transparent;
  color: #FFF;
  font-size: 19px;
  font-weight: bold;
  padding: 7px;
  border-radius: 4px;

  outline: none;
  cursor: pointer;
}
</style>
