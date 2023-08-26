<script setup lang="ts">
import { onMounted, ref, computed } from 'vue';
import { Player } from '../models/Player';
import SelectPlayer from './SelectPlayer.vue';
import Board from './Board.vue';
import ViewPoints from './ViewPoints.vue';

const players = ref<Player[]>([]);
const playerNames = ref({ playerX: '', playerO: '' });
const currentPlayerSymbol = ref('X');
const gameStarted = ref(false);
const winner = ref('');
const showGameOver = ref(false);
const isTie = ref(false);
const gameEnded = ref(false);
const updatePlayers = ref(false);

const squares = ref([
  ['', '', ''],
  ['', '', ''],
  ['', '', ''],
]);

onMounted(() => {
  const savedPlayers = JSON.parse(localStorage.getItem('playersSaved')!);
  if (savedPlayers) {
    players.value = savedPlayers;
  }
});

const handleGameStart = () => {
  if (!gameStarted.value) {
    gameStarted.value = true;

    if (playerNames.value.playerX && playerNames.value.playerO) {
      players.value = [
        new Player(playerNames.value.playerX, 0, 'X'),
        new Player(playerNames.value.playerO, 0, 'O'),
      ];
    }
  }
};

const currentPlayerLabel = computed(() => {
  if (currentPlayerSymbol.value === 'X') {
    return `${playerNames.value.playerX}'s turn`;
  } else {
    return `${playerNames.value.playerO}'s turn`;
  }
});

const handleBoardClick = (row: number, col: number) => {
  if (
    squares.value[row][col] === '' &&
    gameStarted.value &&
    !isGameOver.value
  ) {
    squares.value[row][col] = currentPlayerSymbol.value;
    const newSymbol = currentPlayerSymbol.value === 'X' ? 'O' : 'X';
    currentPlayerSymbol.value = newSymbol;

    handleCheckWinner();
  }
};

const checkWinner = (squares: string[][]) => {
  const winningCombinations: number[][] = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];

  for (let i = 0; i < winningCombinations.length; i++) {
    const [a, b, c] = winningCombinations[i];

    const rowA = a % 3;
    const colA = Math.floor(a / 3);

    const rowB = b % 3;
    const colB = Math.floor(b / 3);

    const rowC = c % 3;
    const colC = Math.floor(c / 3);

    const symbolA = squares[rowA][colA];
    const symbolB = squares[rowB][colB];
    const symbolC = squares[rowC][colC];

    if (symbolA === symbolB && symbolB === symbolC && symbolA !== '') {
      winner.value = symbolA;
      return symbolA;
    }
  }

  return null;
};
const handleCheckWinner = () => {
  const winnerSymbol = checkWinner(squares.value);

  if (winnerSymbol) {
    winner.value = winnerSymbol;

    const winningPlayer = players.value.find(
      (player) => player.symbol === winnerSymbol
    );
    if (winningPlayer) {
      winningPlayer.score++;
    }
  } else {
    const isTieGame = !squares.value.flat().includes('');

    if (isTieGame) {
      isTie.value = true;
      showGameOver.value = true;
      gameStarted.value = false;
    } else {
      showGameOver.value = false;
    }
  }

  if (winnerSymbol || isTie.value) {
    showGameOver.value = true;
    gameStarted.value = false;
    gameEnded.value = true;
    updatePlayers.value = true;

    localStorage.setItem('playersSaved', JSON.stringify(players.value));
  }
};

const isGameOver = computed(
  () => winner.value !== '' || isTie.value || !gameStarted.value
);

const handlePlayAgain = () => {
  winner.value = '';
  isTie.value = false;
  squares.value = [
    ['', '', ''],
    ['', '', ''],
    ['', '', ''],
  ];
  showGameOver.value = false;
  gameStarted.value = true;
  gameEnded.value = false;
};

const resetGame = () => {
  players.value = [
    new Player(playerNames.value.playerX, 0, 'X'),
    new Player(playerNames.value.playerO, 0, 'O'),
  ];
  playerNames.value = { playerX: '', playerO: '' };
  currentPlayerSymbol.value = 'X';
  gameStarted.value = false;
  winner.value = '';
  isTie.value = false;
  gameEnded.value = false;

  squares.value = [
    ['', '', ''],
    ['', '', ''],
    ['', '', ''],
  ];
  localStorage.removeItem('playersSaved');
};
</script>
<template>
  <div>
    <template v-if="!gameStarted && !gameEnded">
      <SelectPlayer
        @game-start="handleGameStart"
        :players="players"
        :player-names="playerNames"
      ></SelectPlayer>
    </template>
    <template v-else>
      <p>{{ currentPlayerLabel }}</p>
      <Board
        :game-started="gameStarted"
        :current-player-symbol="currentPlayerSymbol"
        :squares="squares"
        @update-current-player-symbol="currentPlayerSymbol = $event"
        @check-winner="handleCheckWinner"
        @board-clicked="handleBoardClick"
      ></Board>

      <div v-if="isGameOver" class="gameOver">
        <ViewPoints
          :winner="winner"
          :player-names="playerNames"
          :players="players"
          :is-tie="isTie"
          :show-game-over="showGameOver"
          @play-again-click="handlePlayAgain"
          @reset-game-click="resetGame"
        ></ViewPoints>
      </div>
    </template>
  </div>
</template>

<style scoped>
.gameOver {
  text-align: center;
}
</style>
