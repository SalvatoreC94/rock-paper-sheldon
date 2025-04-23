<template>
  <div class="game">
    <h1>Sasso Carta Forbici Lizard Spock</h1>

    <div class="choices">
      <button 
        v-for="move in moves" 
        :key="move.name" 
        @click="playerChoice(move.name)"
      >
        {{ move.emoji }} {{ move.name }}
      </button>
    </div>

    <div v-if="player && computer" class="result">
      <p>Tu: {{ player }} | Sheldon Cooper: {{ computer }}</p>
      <h2>{{ resultMessage }}</h2>
    </div>

    <div class="score">
      <p>Tu: {{ score.player }} | Sheldon Cooper: {{ score.computer }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const moves = [
  { name: 'Sasso', emoji: '🪨' },
  { name: 'Carta', emoji: '📄' },
  { name: 'Forbici', emoji: '✂️' },
  { name: 'Lizard', emoji: '🦎' },
  { name: 'Spock', emoji: '🖖' },
]

const player = ref(null)
const computer = ref(null)
const result = ref(null)
const score = ref({ player: 0, computer: 0 })

const rules = {
  Forbici: ['Carta', 'Lizard'],
  Carta: ['Sasso', 'Spock'],
  Sasso: ['Lizard', 'Forbici'],
  Lizard: ['Spock', 'Carta'],
  Spock: ['Forbici', 'Sasso'],
}

const phrases = {
  'Forbici-Carta': [
    'Le forbici tagliano la carta… come la logica taglia le emozioni.',
    'Carta? Davvero? Le forbici ridono della tua scelta.'
  ],
  'Carta-Sasso': [
    'La carta avvolge il sasso… proprio come la mia mente avvolge la tua logica.',
    'Il sasso è stato domato da un foglio A4. Che tristezza.'
  ],
  'Sasso-Lizard': [
    'Il sasso schiaccia la lucertola… piccolo rettile, grande sconfitta.',
    'La tua lucertola è ora... un fossile.'
  ],
  'Lizard-Spock': [
    'La lucertola avvelena Spock… persino gli intellettuali hanno un punto debole.',
    'Spock non aveva calcolato la tossicità rettiliana.'
  ],
  'Spock-Forbici': [
    'Spock distrugge le forbici… affascinante.',
    'Logica pura. Addio, forbici.'
  ],
  'Forbici-Lizard': [
    'Le forbici decapitano la lucertola… una scena da non mostrare ai bambini.',
    'Un taglio netto. Letteralmente.'
  ],
  'Lizard-Carta': [
    'La lucertola mangia la carta… condita con un pizzico di vendetta.',
    'La carta? Uno snack per lucertole affamate.'
  ],
  'Carta-Spock': [
    'La carta smentisce Spock… e io smentisco te.',
    'Nemmeno Spock è immune al potere della burocrazia.'
  ],
  'Spock-Sasso': [
    'Spock vaporizza il sasso… *Bazinga!*',
    'Ci vuole un vulcaniano per domare una pietra.'
  ],
  'Sasso-Forbici': [
    'Il sasso schiaccia le forbici… come faccio io con le aspettative degli altri.',
    'Forbici rotte. Vittoria solida.'
  ]
}

function playerChoice(move) {
  player.value = move
  computer.value = randomMove()
  checkWinner()
}

function randomMove() {
  const index = Math.floor(Math.random() * moves.length)
  return moves[index].name
}

function checkWinner() {
  if (player.value === computer.value) {
    result.value = 'draw'
    return
  }

  if (rules[player.value].includes(computer.value)) {
    result.value = 'player'
    score.value.player++
  } else {
    result.value = 'computer'
    score.value.computer++
  }
}

const resultMessage = computed(() => {
  if (!player.value || !computer.value) return ''
  if (result.value === 'draw') return 'Pareggio! Le probabilità erano perfettamente bilanciate... come l’universo dovrebbe essere.'

  const key = `${player.value}-${computer.value}`
  const reverseKey = `${computer.value}-${player.value}`
  const phraseArr = phrases[key] || phrases[reverseKey]
  const phrase = Array.isArray(phraseArr)
    ? phraseArr[Math.floor(Math.random() * phraseArr.length)]
    : phraseArr

  return result.value === 'player'
    ? `${phrase} Hai vinto!`
    : `${phrase} Hai perso!`
})
</script>

<style scoped>
.game {
  text-align: center;
  font-family: 'Courier New', monospace;
  padding: 2rem;
}

.choices button {
  margin: 0.5rem;
  padding: 1rem 1.5rem;
  font-size: 1.2rem;
  cursor: pointer;
  border: none;
  background-color: #0B0202FF;
  border-radius: 8px;
  transition: transform 0.1s ease;
}

.choices button:hover {
  transform: scale(1.05);
  background-color: #030325FF;
}

.result {
  margin-top: 2rem;
  font-size: 1.5rem;
}

.score {
  margin-top: 1rem;
  font-size: 1.2rem;
}
</style>
