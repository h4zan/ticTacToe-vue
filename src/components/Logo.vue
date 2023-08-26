<script setup lang="ts">
import logoLink from '../assets/tic-tac-toe_1021264.png';

const logo = logoLink;
const gameTitle = 'Tic Tac Toe';

const getLetterStyle = (index: number) => ({
  color: index % 2 === 0 ? '#0086f4' : ' #e94141',
});
</script>
<template>
  <div class="logoWrapper">
    <img :src="logo" alt="An illustration of Tictactoe game." class="logo" />
    <h2 class="color-alternate">
      <span
        v-for="(letter, index) in gameTitle"
        :key="index"
        :style="getLetterStyle(index)"
      >
        {{ letter }}
      </span>
    </h2>
  </div>
</template>
<style scoped>
.logoWrapper {
  display: flex;
  align-items: center;
}
.logo {
  height: 2em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

.color-alternate {
  display: inline-block;
}

.color-alternate > span:nth-child(odd) {
  color: #0086f4;
}

.color-alternate > span:nth-child(even) {
  color: #e94141;
}
</style>
