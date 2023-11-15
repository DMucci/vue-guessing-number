<script setup>

import { ref } from 'vue';
import { computed } from '@vue/reactivity';

const randomNumber = ref(parseInt(Math.random() * 100 + 1)) //en lugar de parseInt se puede pone Math.floor para poner un num aleatorio entre 0 y 100

const isPlaying = ref(false)

const previousGuesses = ref([])

const currentGuess = ref('')

const MAX_ATTEMPS = 10
const attempts = ref(MAX_ATTEMPS)

const checkGuess = () => {
  previousGuesses.value.push(currentGuess.value)
  currentGuess.value = ('')
  attempts.value--

}
// const lastNumber = () => {
//   return previousGuesses.value[previousGuesses.value.length-1]
// }

const lastNumber = computed(() => {
  return previousGuesses.value[previousGuesses.value.length - 1]
})
const mustDisableInput = computed (() => {
  return attempts.value==0 || lastNumber.value==randomNumber.value
})

const startGame = () => {
  isPlaying.value = true
  console.log("Game Starts")
}

const restartButton = computed (() => {
  if (attempts.value==0 || lastNumber.value==randomNumber.value)
  return restartButton

  //SE PODRIA CREAR UNA VARIABLE QUE DIGA isGameFinished y que sirva tanto para el restartButton como para el musDisableInput pq es lo mismo
})

const restartGame = () => {
  randomNumber.value = ref(parseInt(Math.random() * 100 + 1))
  previousGuesses.value = []
  attempts.value = MAX_ATTEMPS
}

// * REto: mensajes de has ganado o has perdido
//   *
//   * 1. ¿Cómo sabemos que hemos perdido? Cuando attempts es 0 Y mi ultimo intento también ha fallado
//   * 2. ¿Cómo sabemos que he ganado? Si mi último intento (lastNumber) es igual al número a adivinar, he ganado

</script>

<template>
  <main>
    <h2>Number guessing game</h2>

    <section v-if="isPlaying">
      <p>Try and guess a random number between 1 and 100.</p>
      <p>You have 10 attempts to guess the right number.</p>
      <div id="wrapper">
        <label for="guessField">Guess a number</label>
        <input :disabled="mustDisableInput" v-model="currentGuess" type="number" id="guessField">
        <button :disabled="!currentGuess" @click="checkGuess" class="button-check">Check Guess</button>

        <div v-show="lastNumber" class="resultParas">
          <p>Previous Guesses: <span class="guesses">{{ previousGuesses.join("-") }}</span></p>
          <p>Last number checked: {{ lastNumber }}</p>
          <!-- Al ser computed property va sin los parentesis  -->
          <p>Guesses Remaining: <span class="lastResult">{{ attempts }}</span></p>
          <p v-if="attempts == 0 && lastNumber != randomNumber">¡Has perdido! El número a adivinar era: {{ randomNumber }} </p>
          <p v-else-if="lastNumber == randomNumber">¡Felicidades! ¡Has ganado!</p>
          <p v-else-if="randomNumber > lastNumber" style="color: green">The number needs to be higher</p>
          <p v-else style="color: red">The number needs to be lower</p>

          <button @click="restartGame" v-show="restartButton"> Restart Game! </button>

        </div>

      </div>
    </section>
    <section v-else>

      <button @click="startGame()">Start Game</button>
    </section>
  </main>
</template>

<style>
.button-check {
  background-color: rgb(128, 70, 128);
}
</style>
