<script setup lang="ts">
interface ISquareProps {
  value: string;
  row: number;
  col: number;
}
const squareProps = defineProps<ISquareProps>();

const emitSquare = defineEmits(['square-clicked']);

const handleClick = () => {
  if (squareProps.value === '') {
    emitSquare('square-clicked', squareProps.row, squareProps.col);
  }
};
</script>

<template>
  <div
    class="square"
    :class="{ X: value === 'X', O: value === 'O' }"
    @click="handleClick"
  >
    {{ value }}
  </div>
</template>

<style scoped>
.square {
  width: 80px;
  height: 80px;
  border: 1px solid #928e8e;
  box-shadow: #63636333 0px 2px 8px 0px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24px;
  font-weight: bold;
  cursor: pointer;
  border-radius: 5px;
  margin-bottom: 5px;
}

.X {
  color: #e94141;
}

.O {
  color: #0086f4;
}
</style>
