<script setup lang="ts">
import Square from './Square.vue';

interface IBoardProps {
  gameStarted: boolean;
  currentPlayerSymbol: string;
  squares: string[][];
}

const boardProps = defineProps<IBoardProps>();

const emitBoard = defineEmits(['board-clicked']);

const handleSquareClick = (row: number, col: number) => {
  if (boardProps.squares[row][col] === '' && boardProps.gameStarted) {
    emitBoard('board-clicked', row, col);
  }
};
</script>

<template>
  <div class="board">
    <div class="row" v-for="(row, rowIndex) in squares" :key="rowIndex">
      <Square
        v-for="(value, colIndex) in row"
        :key="colIndex"
        :value="value"
        :row="rowIndex"
        :col="colIndex"
        @square-clicked="handleSquareClick"
      />
    </div>
  </div>
</template>

<style scoped>
.board {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 5px;
}

.row {
  display: flex;
  flex-wrap: wrap;
  flex-direction: column;
}
</style>
