<script setup>
import { ref, onMounted } from "vue";
const playerswithScores = ref([]);
let theresaplayer = ref(false);

setInterval(() => {
  //save to local storage
  if (playerswithScores.value.length > 0) {
    localStorage.setItem(
      "playerswithScores",
      JSON.stringify(playerswithScores.value)
    );
    console.log("saved");
    console.log(playerswithScores.value);
  }
}, 1000);
//load from local storage on mount
onMounted(() => {
  if (localStorage.getItem("playerswithScores")) {
    playerswithScores.value = JSON.parse(
      localStorage.getItem("playerswithScores")
    );
    theresaplayer.value = true;
  }
});

const playerName = ref("");
function addPlayer() {
  playerswithScores.value.push({ name: playerName.value, scores: [] });
  playerName.value = "";
  theresaplayer = true;
}

function addScorestoPlayers() {
  playerswithScores.value.forEach((player) => {
    player.scores.push(0);
  });
  console.log(playerswithScores.value);
}
function removeLastPlayer() {
  playerswithScores.value.pop();
}

function restartGame() {
  playerswithScores.value.forEach((player) => {
    player.scores = [];
  });
}

function removeEverything() {
  playerswithScores.value = [];
  theresaplayer.value = false;
}
</script>

<template>
  <div class="flex gap-2">
    <input class="m-2" type="text" v-model="playerName" />
    <button @click="addPlayer">Add player</button>
    <button @click="removeLastPlayer">Remove last player</button>
    <button @click="restartGame">Restart game</button>
    <button @click="removeEverything">Remove everything</button>
  </div>

  <div v-if="theresaplayer">
    <table class="w-full">
      <tr>
        <th>Round</th>
        <th v-for="player in playerswithScores">{{ player.name }}</th>
      </tr>

      <tr v-for="(round, i) in playerswithScores[0].scores">
        <td>Round {{ i + 1 }}</td>
        <td v-for="player in playerswithScores">
          <input type="number" v-model="player.scores[i]" class="w-full" />
        </td>
      </tr>
      <tr>
        Sum scores
        <td v-for="player in playerswithScores">
          {{ player.scores.reduce((a, b) => a + b, 0) }}
        </td>
      </tr>
      <tr>
        <button @click="addScorestoPlayers">Add round</button>
      </tr>
    </table>
  </div>
</template>
