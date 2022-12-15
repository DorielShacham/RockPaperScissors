<script setup>
import { ref, computed, onMounted } from 'vue'

const CHOICES = ['rock', 'paper', 'scissors']

const wins = ref(0)
const losses = ref(0)
const draws = ref(0)

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
    scissors: 'draw',
  }
}

const WinPercentage = computed(() => {
  const total = wins.value + losses.value + draws.value
  console.log(total)
  return total ? Math.round((wins.value / total) * 100) : 0
})

const play = c => {
  choice.value = c

  const randomNum = Math.floor((Math.random() * 10) % CHOICES.length)
  computerChoice.value = CHOICES[randomNum]

  const outcome = outcomes[c][computerChoice.value]

  if (outcome === 'win') {
    wins.value++
    verdict.value = 'You win!'
  } else if (outcome === 'loss') {
    losses.value++
    verdict.value = 'You lose!'
  } else {
    draws.value++
    verdict.value = 'It is a Draw!'
  }

  SavedGame()
}

const SavedGame = () => {
  localStorage.setItem('wins', wins.value)
  localStorage.setItem('losses', losses.value)
  localStorage.setItem('draws', draws.value)
}

const LoadGame = () => {
  wins.value = parseInt(localStorage.getItem('wins')) || 0
  losses.value = parseInt(localStorage.getItem('losses')) || 0
  draws.value = parseInt(localStorage.getItem('draws')) || 0
}

const ResetRound = () => {
  CHOICES.value = null;
  computerChoice.value = null;
  verdict.value = null;
}

onMounted(() => {
  LoadGame()
  window.addEventListener('keypress', e => {
    if (e.key === 'r') { //reset the game oncee the user presses 'r'
      ResetRound()
    }
  })
})

</script>

<template>
  <div class="bigContainer"><!--//bg-gray-700 text-white text-center min-h-screen flex flex-col>-->
    <header class="container"><!--//container mx-auto p-6-->
      <h1 class="text">Rock, Paper, Scissors</h1><!--//text-4xl font-bold-->
    </header>
  </div>

  <main class="containerMain"><!--//mx-auto p6 flex-1-->
    <div  class="if"><!--//flex item-center justify-center mx-6-->
     <button 
     placeholder='rock'
       @click="play('rock')"
       class="rock"><!--//bg-white rounded-full shadow-1g w-64 p-12 mx-6 transition-colors duration-300 hover:bg-pink-500-->
       <img src="./assets/rock.jpeg" alt="Rock" class="w-full"/>
      </button>

      <button 
       @click="play('paper')"
        class="paper"><!--//bg-white rounded-full shadow-1g w-64 p-12 mx-6 transition-colors duration-300 hover:bg-green-500-->
       <img src="./assets/paper.jpeg" alt="Paper" class="w-full"/>
      </button>

     <button 
      @click="play('scissors')"
       class="scissors"><!--//bg-white rounded-full shadow-1g w-64 p-12 mx-6 transition-colors duration-300 hover:bg-yellow-500-->
       <img src="./assets/scissors.jpeg" alt="Scissors" class="w-full"/>
     </button>

   </div>

   <div >
    <div class="mainPlayerChoice"><!--text-3xl mb-4-->
      You picked <span class="choicePlayer">{{ choice }}</span><!--text-pink-500-->
    </div>

    <div class="mainComputerChoice"><!--text-3xl mb-4-->
      The computer picked <span class="choiceComputer">{{ computerChoice }}</span><!--text-green-500-->
    </div>

    <div class="verdict"><!--text-6xl mb-12-->
      {{ verdict }}
    </div>
  </div>

  <button @click="ResetRound" class="resetButton">Reset</button><!--bg-pink-500 text-lg py-2 px-4-->

  <div class="winslossesdraws"><!--mt-12 text-3xl mb-4-->
    {{ wins }} : {{ losses }} : {{ draws }}
  </div>

  <div class="winPercentage"><!--text-lg-->
    Win Percentage: {{ Math.round(WinPercentage)}}%
  </div>

  </main>
</template>
