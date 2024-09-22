<template>
  <div class="flex">
    <header class="header">
      <h1>ROCK PAPER SCISSORS</h1>
    </header>
    <main class="main">
      <div>
        <h2>Round: {{ round }}</h2>
      </div>
      <div class="scores">
        <div class="score">
          <h2>Player</h2>
          <div class="result">{{ playerScore }}</div>
        </div>
        <div class="score">
          <h2>Computer</h2>
          <div class="result">{{ computerScore }}</div>
        </div>
        <div class="score">
          <h2>Draw</h2>
          <div class="result">{{ draw }}</div>
        </div>
      </div>
      <div class="icons">
        <div class="icon-background" @click="handlePlayerChoice($event)" data-choice="rock">
          <IconRock></IconRock>
        </div>
        <div class="icon-background" @click="handlePlayerChoice($event)" data-choice="paper">
          <IconPaper></IconPaper>
        </div>
        <div class="icon-background" @click="handlePlayerChoice($event)" data-choice="scissors">
          <IconScissors></IconScissors>
        </div>
      </div>
      <div class="game-result">
        <p>{{ gameResult }}</p>
        <p>{{ winner }}</p>
      </div>
      <div v-if="isGameOver">
        <input type="button" value="Start a new game" @click="startNewGame()">
      </div>
    </main>
  </div>
</template>

<script lang="ts">
import IconPaper from './icons/IconPaper.vue';
import IconRock from './icons/IconRock.vue';
import IconScissors from './icons/IconScissors.vue';
import { ref } from 'vue'

export default {
  components: {
    IconScissors,
    IconRock,
    IconPaper
  },
  setup() {
    let round = ref<number>(0)
    let computerScore = ref<number>(0)
    let playerScore = ref<number>(0)
    let draw = ref<number>(0)
    let gameResult = ref<string>('')
    let winner = ref<string>('')
    let isGameOver = ref<boolean>(false)
    let restartGame = ref<boolean>(false)

    const handleComputerChoice = () => {
      const options: string[] = ['rock', 'paper', 'scissors'] 
      const random = Math.floor(Math.random() * 3)
      return options[random]
    }

    const handlePlayerChoice = (e: any) => {
      const computerChoice = handleComputerChoice()
      const playerChoice = e.currentTarget.dataset.choice
      
      
      if (round.value >= 5) {
        return
      }

      round.value++

      if (playerChoice === computerChoice) {
        gameResult.value = 'Draw'
        draw.value++
      }
      if (playerChoice === 'scissors') {
        if (computerChoice === 'rock') {
          gameResult.value = `You lose! ${computerChoice} beats ${playerChoice}`
          computerScore.value++
        } else if (computerChoice === 'paper') {
          gameResult.value = `You win! ${playerChoice} beats ${computerChoice}`
          playerScore.value++
        }
      }
      if (playerChoice === 'paper') {
        if (computerChoice === 'scissors') {
          gameResult.value = `You lose! ${computerChoice} beats ${playerChoice}`
          computerScore.value++
        } else if (computerChoice === 'rock') {
          gameResult.value = `You win! ${playerChoice} beats ${computerChoice}`
          playerScore.value++
        }
      }
      if (playerChoice === 'rock') {
        if (computerChoice === 'paper') {
          gameResult.value = `You lose! ${computerChoice} beats ${playerChoice}`
          computerScore.value++
        } else if (computerChoice === 'scissors') {
          gameResult.value = `You win! ${playerChoice} beats ${computerChoice}`
          playerScore.value++
        }
      }

      if (round.value >= 5) {
        playerScore.value === computerScore.value
        ? winner.value = 'The game ends in a draw'
        : playerScore.value > computerScore.value
          ? winner.value = 'Player wins'
          : winner.value = 'Computer wins'
        
        gameOver()
        return
      }
    }

    function gameOver() {
      isGameOver.value = true
    }

    function startNewGame() {
      restartGame.value = !restartGame.value
      if(restartGame) {
        resetGame()
        restartGame.value = false
        isGameOver.value = false
      }
    }

    function resetGame() {
      round.value = 0
      playerScore.value = 0
      computerScore.value = 0
      draw.value = 0
      gameResult.value = ''
      winner.value = ''
    }

    return {
      round,
      computerScore,
      playerScore,
      draw,
      gameResult,
      winner,
      isGameOver,
      startNewGame,
      handlePlayerChoice
    }
  }
}
</script>

<style scoped>
.flex {
  display: flex;
  flex-direction: column;
  gap: 50px;
}
.header {
  display: flex;
  justify-content: center;
  background-color: #2d2d2d;
  padding: 20px;
}
.main {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 50px;
}
.scores {
  display: flex;
  gap: 80px;
}
.score {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.result {
  font-size: 30px;
}
.icons {
  display: flex;
  gap: 50px;
  @media (max-width: 480px) {
    flex-direction: column;
    align-items: center;
  }
}
.icon-background {
  background-color: white;
  border-radius: 10px;
  padding: 10px;
  cursor: pointer;
}
</style>
