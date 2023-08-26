<script setup lang="ts">
import { ref } from 'vue';
import { Player } from '../models/Player';

interface ISelectPlayerProps {
  players: Player[];
  playerNames: {
    playerX: string;
    playerO: string;
  };
}
const playerName = ref('');

const props = defineProps<ISelectPlayerProps>();

const emit = defineEmits(['game-start']);

const enterPlayers = () => {
  if (!props.playerNames.playerX) {
    props.playerNames.playerX = playerName.value.trim().toUpperCase();
    playerName.value = '';
  } else if (!props.playerNames.playerO) {
    props.playerNames.playerO = playerName.value.trim().toUpperCase();
    playerName.value = '';
  }

  if (props.playerNames.playerX && props.playerNames.playerO) {
    emit('game-start', {
      playerNames: {
        X: props.playerNames.playerX,
        O: props.playerNames.playerO,
      },
    });
    savePlayersToLS(props.players);
  }
};

const savePlayersToLS = (players: Player[]) => {
  localStorage.setItem('playersSaved', JSON.stringify(players));
};
</script>

<template>
  <div>
    <p>Enter names to start the game.</p>

    <form @submit.prevent="enterPlayers" class="enter-players">
      <label
        v-if="!playerNames.playerX && !playerNames.playerO"
        class="player-label"
        >Enter name for player X:</label
      >
      <label
        v-if="playerNames.playerX && !playerNames.playerO"
        class="player-label"
        >Enter name for player O:</label
      >

      <input
        type="text"
        v-model="playerName"
        :placeholder="
          playerNames.playerX
            ? 'Enter name for player O'
            : 'Enter name for player X'
        "
        class="player-input"
        maxlength="15"
      />
      <button :disabled="!playerName.trim()" type="submit" class="submit-btn"
        >Submit</button
      >
    </form>
  </div>
</template>

<style scoped>
.enter-players {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 20px;
}

.player-label {
  font-size: 1.2rem;
  margin-bottom: 10px;
  font-weight: 600;
}

.players-label {
  font-size: 1.2rem;
  margin-top: 10px;
  margin-bottom: 10px;
}

.player-input {
  padding: 10px;
  border: 2px solid #7e7cc5;
  box-shadow: #63636333 0px 2px 8px 0px;
  border-radius: 5px;
  font-size: 1.2rem;
  margin-bottom: 20px;
  margin-top: 20px;
  width: 100%;
  max-width: 300px;
  background-color: ghostwhite;
}

.submit-btn {
  background-color: #6495ed;
  color: #fff;
  border: none;
  border-radius: 5px;
  padding: 10px 20px;
  font-size: 1.2rem;
  cursor: pointer;
}

.submit-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
  color: #ff0000;
}
</style>
