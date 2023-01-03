<script setup>
import{ ref, computed, onMounted } from 'vue'

const wins = ref(0)
const draws = ref(0)
const losses = ref(0)

const choice = ref(null)
const computerChoice = ref(null)
const verdict = ref(null)

const outcomes = {
  rock: {
    rock: 'draw',
    paper: 'loss',
    scissors: 'win',
  },
  paper: {
    rock: 'win',
    paper: 'draw',
    scissors: 'loss',
  },
  scissors: {
    rock: 'loss',
    paper: 'win',
    scissors: 'paper',
  }
}

const winPercentage =  computed(() => {
  const total = wins.value + draws.value + losses.value
  return total ? (wins.value / total) * 100 : 0 
})


const play = c => {
  choice.value = c 
  const choices = ['rock', 'paper', 'scissors']
  const random = Math.floor(Math.random() * choices.length)
  computerChoice.value = choices[random]

  const outcome = outcomes[c][computerChoice.value]

  if (outcome === 'win') {
    wins.value++
    verdict.value = 'You win!'
  } else if (outcome === 'loss') {
    losses.value++
    verdict.value = 'You lose!'
  } else {
    draws.value++
    verdict.value = 'Draw!'
  }

  SaveGame()
}


const SaveGame = () => {
  localStorage.setItem('wins', wins.value) || 0
  localStorage.setItem('draws', draws.value) || 0
  localStorage.setItem('losses', losses.value) || 0
}

const LoadGame = () => {
  wins.value = parseInt(localStorage.getItem('wins')) || 0
  draws.value = parseInt(localStorage.getItem('draws')) || 0
  losses.value = parseInt(localStorage.getItem('losses')) || 0
}

const ResetRound = () => {
  choice.value = null
  computerChoice.value = null
  verdict.value = null
}

onMounted(() => {
  LoadGame()
  window.addEventListener('keypress', e => {
    if (e. key === 'r') {
      ResetRound()
    }
  })
})



</script>

<template>
  <div class= "bg-gray-700 text-white text-center min-h-screen flex flex-col">
    <header class='container mx-auto p-6'>
        <h1 class="text-4xl"> <a class="text-pink-400">Rock</a>, <a class="text-green-400">Paper</a>, <a class="text-orange-400">Scissors</a> </h1>
  
      
    </header>

    <main class="container mx-auto p-6 flex-1">
      <div v-if="choice === null" class="flex items-center justify-center -mx-6">


          <button @click="play('rock')" 
            class="bg-pink-400 rounded-full shadow-lg w-70 p-6 mx-3 md:p-12 md:mx-6
            transition-colors duration-300 hover:bg-white">
            <img src="./assets/RockIcon.svg" alt="Rock" class="w-full" />
            <!-- <h1 class="block md:hidden lg:hidden">ROCK</h1> -->
            
          </button>

          <button @click="play('paper')" 
            class="bg-green-400 rounded-full shadow-lg w-70 p-6 mx-3
             md:p-12 md:mx-6
            transition-colors duration-300 hover:bg-white">
            <img src="./assets/PaperIcon.svg" alt="Paper" />
          </button>

          <button @click="play('scissors')" 
            class="bg-orange-400 rounded-full shadow-lg w-70 p-6 mx-3 md:p-12 md:mx-6
            transition-colors duration-300 hover:bg-white">
            <img src="./assets/ScissorsIcon.svg" alt="Scissors" class="w-full" />
          </button>
      </div>
      <div v-else>
        <div class="text-3xl mb-4">
          You picked <span class="text-pink-500">{{ choice }}</span>
        </div>
        <div class="text-3xl mb-4">
          The robot picked <span class="text-green-500">{{ choice }}</span>
        </div>
        <div class="text-6xl mb-12">
          {{ verdict }}
        </div>
        <button @click="ResetRound" class="bg-pink-500 text-lg py-2 px-4">Reset</button>
      </div>

      <div class="mt-12 text-3xl mb-4">
        {{ wins }} :  {{ draws }} :  {{ losses }} 
      </div>

      <div class="text-lg">
        Win rate: {{Math.round(winPercentage)}}%
      </div>
    </main>
  </div>
</template>
 
