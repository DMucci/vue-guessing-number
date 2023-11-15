<script setup>

import { computed } from '@vue/reactivity';
import { ref } from 'vue';

const isPlaying = ref(false)
const maxNumber = ref(100)
const randomNumber = ref(0)
const previousGuesses = ref([])
const currentGuess = ref("")
const attempts = ref(3)

const startGame = () => {
  console.log("Game Starts")
  randomNumber.value = parseInt(Math.random() * maxNumber.value) + 1
  console.log("Game to guess: ", randomNumber.value)
  isPlaying.value = true;
}

const checkGuess = () => {
  console.log("Check guess: ", currentGuess.value, randomNumber.value)
  previousGuesses.value.push(currentGuess.value)
  currentGuess.value = ""
  attempts.value--
}

const difficultyRatio = computed(() => {
  const ratio = maxNumber.value / attempts.value
  if (ratio > 10) {
    return "Hard"
  } else if (ratio < 10) {
    return "Easy"
  }

  return "Normal"
})

const mustDisable = computed(() => {
  return attempts.value == 0 || previousGuesses[previousGuesses.value.length - 1] == randomNumber.value || !currentGuess.value
})

const lastGuess = computed(() => {
  return previousGuesses.value[previousGuesses.value.length - 1]
})

</script>

<template>
  <main>
    <!-- check-->
    <h2>Number guessing game</h2>

    <section v-if="isPlaying">
      <p>Try and guess a random number between 1 and {{ maxNumber }}.</p>
      <p>You have {{ attempts }} attempts to guess the right number.</p>
      <p>Your difficulty ratio is {{ difficultyRatio }}</p>
      <div id="wrapper">
        <div>
          <label for="guessField">Guess a number</label>
          <input v-model="currentGuess" type="number" id="guessField" class="guessField">
          <button @click="checkGuess" :disabled="mustDisable" class="button-check">Check Number</button>
        </div>
        <div v-show="previousGuesses.length != 0">
          <p>Previous Guesses: <span>{{ previousGuesses.join("-") }}</span></p>
          <p>Guesses Remaining: <span class="lastResult"> {{ attempts }}</span></p>
          <p v-if="attempts == 0 && lastGuess != randomNumber"> YOU LOST 🤧</p>
          <p style="color: red" v-else-if="lastGuess > randomNumber"> Correct number is
            lower &#8595;
          </p>
          <p style="color: green" v-else-if="lastGuess < randomNumber"> Correct number
            is
            higher &#x2191;</p>
          <p v-else-if="lastGuess == randomNumber" style="font-size: 48px;">You Won!!
            Correct number was {{
              randomNumber }} </p>
        </div>
      </div>
    </section>
    <section v-else>
      <label for="maxNumber">Max Number to Guess</label>
      <input v-model="maxNumber" type="number" id="maxNumber">
      <label for="attemps">Max Number of attemps to guess the number</label>
      <input v-model="attempts" type="number" name="" id="attemps">
      <button @click="startGame">Start Game</button>
    </section>
  </main>
</template>

<style>
.button-check {
  background-color: rgb(128, 70, 128);
}
</style>