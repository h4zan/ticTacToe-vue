<script setup lang="ts">
import { Player } from '../models/Player';

interface IViewPointsProps {
  winner: String;
  isTie: Boolean;
  showGameOver: Boolean;
  players: Player[];
  playerNames: {
    playerX: string;
    playerO: string;
  };
}
const viewPointsProps = defineProps<IViewPointsProps>();

const emitViewPoints = defineEmits();

const xSymbol = '❌';
const oSymbol = '🔵';
const tieSymbol = '🏳️';

const handlePlayAgainClick = () => {
  emitViewPoints('play-again-click', viewPointsProps.players);
};

const handleResetGameClick = () => {
  emitViewPoints('reset-game-click', viewPointsProps.players);
};
</script>
<template>
  <div class="displayGameOver">
    <h3>Game Over</h3>
    <template v-if="winner">
      <span :style="{ color: winner === 'X' ? 'red' : 'blue' }">
        {{
          winner === 'X'
            ? xSymbol + ' ' + playerNames.playerX
            : oSymbol + ' ' + playerNames.playerO
        }}
      </span>
      <span> wins!</span>
    </template>
    <template v-else-if="isTie && showGameOver">
      <p>{{ tieSymbol }} It's draw, try again! </p>
    </template>

    <template v-for="player in viewPointsProps.players">
      <p> {{ player.name }}: {{ player.score }} </p>
    </template>

    <div class="resetBtns">
      <form @submit.prevent="handlePlayAgainClick">
        <button type="submit" class="reset">Play Again</button>
      </form>
      <form @submit.prevent="handleResetGameClick">
        <button type="submit" class="reset">New Game</button>
      </form>
    </div>
  </div>
</template>

<style scoped>
.displayGameOver {
  font-size: 1.2rem;
}
.resetBtns {
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
  margin-top: 2rem;
}

button {
  background-color: cornflowerblue;
  color: #fff;
  border: none;
  border-radius: 5px;
  padding: 8px 8px;
  font-size: 1.2rem;
  cursor: pointer;
  margin-top: 1rem;
}
</style>
