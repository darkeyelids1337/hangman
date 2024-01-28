<script setup lang="ts">
import { ref, watch } from "vue";
import GameHeader from "./components/GameHeader.vue";
import GameFigure from "./components/GameFigure.vue";
import GameWrongLetters from "./components/GameWrongLetters.vue";
import GameWord from "./components/GameWord.vue";
import GameNotification from "./components/GameNotification.vue";
import GamePopup from "./components/GamePopup.vue";
import { useRandomWord } from "./composables/useRandomWord";
import { useLetters } from "./composables/useLetters";
const { word, getRandomWord } = useRandomWord();
const {letters, correctLetters, wrongLetters, isLose, isWin, addLetter, resetLetters} = useLetters(word);

const notification = ref<InstanceType<typeof GameNotification> | null>(null);
const popup = ref<InstanceType<typeof GamePopup> | null>(null);


watch(wrongLetters, () => {
  if (isLose.value) {
    popup.value?.open("lose");
  }
});

watch(correctLetters, () => {
  if (isWin.value) {
    popup.value?.open("win");
  }
});
window.addEventListener("keydown", ({ key }) => {
  if (isLose.value || isLose.value) {
    return;
  }
  if (letters.value.includes(key)) {
    notification.value?.open();
    setTimeout(() => notification.value?.close(), 2000);
    return;
  }
  addLetter(key)
});

const restart = () => {
  getRandomWord();
  resetLetters();
  popup.value?.close();
};
</script>

<template>
  <GameHeader></GameHeader>
  <div class="game-container">
    <GameFigure :wrongLettersCount="wrongLetters.length"></GameFigure>
    <GameWrongLetters :wrongLetters="wrongLetters"></GameWrongLetters>
    <GameWord :word="word" :correctLetters="correctLetters"></GameWord>
  </div>
  <GamePopup ref="popup" :word="word" @restart="restart"></GamePopup>
  <GameNotification ref="notification"></GameNotification>
</template>

<style scoped></style>
