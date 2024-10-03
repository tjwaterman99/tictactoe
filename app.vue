<template>
  <div class="container mx-auto max-w-2xl bg-gray-50 p-4">
  <div class="flex flex-row">
    <div v-for="player of game.players" class="border border-black-100 w-1/2 bg-gray-400 text-gray-800 p-4 font-bold" :class="[playerColor(player)]">
      <p class="text-center">{{ player.name }} [{{ player.symbol }}]</p>
    </div>
  </div>

  <div class="flex justify-center mt-16">
    <div class="grid gap-1 grid-cols-3">
      <div v-for="(piece, index) of game.pieces" class="border-2 border-gray-600 w-16 h-16 align-middle flex justify-center" @click="play(index)" :class="{isWinningPiece: isWinningPiece(index)}">
        <div class="text-4xl text-center align-baseline">{{ piece.player?.symbol}}</div>
      </div>
    </div>
  </div>

  <div class="flex justify-center mt-16">
    <button @click="restart" class="bg-pink-800 px-4 py-2 rounded-lg font-bold text-white">Restart</button>
  </div>
</div>
</template>

<script setup lang="ts">
import type { Player } from '@tomwaterman/tictactoets'
import { createGame } from '@tomwaterman/tictactoets'

let game = ref(createGame('Bob', 'Hannah'))

function play(index: number) {
  if (game.value.isFinished().over) {
    return
  } else {
    game.value.play(index)
  }
}

function isWinningPiece(index: number) {
  let status = game.value.isFinished()
  if (status.winning_pieces) {
    return status.winning_pieces.includes(game.value.pieces[index])
  } else {
    return false
  }
}

function playerColor(player: Player) {
  let status = game.value.isFinished()
  if (!status.over) {
    if (game.value.currentPlayer() == player) {
      return "bg-green-400"
    } else {
      return "bg-gray-200"
    }
  } else {
    if (status.winning_player == player) {
      return "bg-yellow-400"
    } else {
      return "bg-gray-200"
    }
  }
}

function restart() {
  game.value.restart()
}

console.log("Loaded")
</script>

<style scoped>
.isWinningPiece {
  @apply bg-yellow-400
}
</style>