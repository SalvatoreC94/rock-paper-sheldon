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
      <p>Tu: {{ player }} | CPU: {{ computer }}</p>
      <h2>{{ resultMessage }}</h2>
    </div>

    <div class="score">
      <p>Tu: {{ score.player }} | CPU: {{ score.computer }}</p>
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
    'Carta? Davvero? Le forbici ridono della tua scelta.',
    'ZAC! La tua carta è ora materiale da riciclo.'
  ],
  'Carta-Sasso': [
    'La carta avvolge il sasso… proprio come la mia mente avvolge la tua logica.',
    'Il tuo sasso è stato domato da un foglio A4. Che tristezza.',
    'Sasso sconfitto dalla burocrazia. Ironico, no?'
  ],
  'Sasso-Lizard': [
    'Il sasso schiaccia la lucertola… piccolo rettile, grande sconfitta.',
    'La tua lucertola è ora... un fossile.',
    'La paleontologia è servita!'
  ],
  'Lizard-Spock': [
    'La lucertola avvelena Spock… persino gli intellettuali hanno un punto debole.',
    'Spock non aveva calcolato la tossicità rettiliana.',
    'Il rettile colpisce ancora!'
  ],
  'Spock-Forbici': [
    'Spock distrugge le forbici… affascinante.',
    'Logica pura. Addio, forbici.',
    'Il potere vulcaniano vince ancora.'
  ],
  'Forbici-Lizard': [
    'Le forbici decapitano la lucertola… scena non adatta ai deboli di cuore.',
    'Un taglio netto. Letteralmente.',
    'La biologia ha perso oggi.'
  ],
  'Lizard-Carta': [
    'La lucertola mangia la carta… condita con un pizzico di vendetta.',
    'La carta? Uno snack per lucertole affamate.',
    'Carta riciclata… nel senso più gustoso.'
  ],
  'Carta-Spock': [
    'La carta smentisce Spock… e io smentisco te.',
    'Nemmeno Spock è immune al potere della burocrazia.',
    'Una pagina di verità sconfigge l’intergalattico.'
  ],
  'Spock-Sasso': [
    'Spock vaporizza il sasso… *Bazinga!*',
    'Ci vuole un vulcaniano per domare una pietra.',
    'La logica ha polverizzato la tua forza bruta.'
  ],
  'Sasso-Forbici': [
    'Il sasso schiaccia le forbici… come faccio io con le aspettative degli altri.',
    'Forbici rotte. Vittoria solida.',
    'La fisica batte la geometria tagliente.'
  ]
}

const player = ref(null)
const computer = ref(null)
const result = ref(null)
const score = ref({ player: 0, computer: 0 })

function playerChoice(move) {
  player.value = move
  computer.value = getRandomMove()
  determineResult()
}

function getRandomMove() {
  const index = Math.floor(Math.random() * moves.length)
  return moves[index].name
}

function determineResult() {
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

  if (result.value === 'draw') {
    return 'Pareggio! Le probabilità erano perfettamente bilanciate... come l’universo dovrebbe essere.'
  }

  const combo = `${player.value}-${computer.value}`
  const reverseCombo = `${computer.value}-${player.value}`
  const phraseList = phrases[combo] || phrases[reverseCombo] || ['Una mossa sorprendente!']
  const randomPhrase = phraseList[Math.floor(Math.random() * phraseList.length)]

  return result.value === 'player'
    ? `${randomPhrase} Hai vinto!`
    : `${randomPhrase} Hai perso!`
})
</script>

<style scoped>
.game {
  text-align: center;
  font-family: 'Segoe UI', 'Courier New', monospace;
  padding: 2rem;
  background: linear-gradient(to bottom, #f6f9fc, #e3eaf3);
  border-radius: 16px;
  max-width: 700px;
  margin: 3rem auto;
  box-shadow: 0 0 30px rgba(0, 0, 0, 0.08);
}

h1 {
  font-size: 2.2rem;
  margin-bottom: 2rem;
  color: #1a1a1a;
}

.choices {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 1rem;
  margin-bottom: 2rem;
}

.choices button {
  padding: 1rem 1.4rem;
  font-size: 1.1rem;
  font-weight: bold;
  cursor: pointer;
  border: 2px solid transparent;
  background: #ffffff;
  color: #333;
  border-radius: 12px;
  transition: 0.2s ease;
  box-shadow: 0 3px 8px rgba(0, 0, 0, 0.1);
}

.choices button:hover {
  background-color: #d0e0ff;
  transform: translateY(-2px);
  border-color: #6c8cff;
}

.result {
  margin-top: 1rem;
  font-size: 1.4rem;
  color: #1a1a1a;
}

.result p {
  font-size: 1.1rem;
  margin-bottom: 0.5rem;
  color: #555;
}

.result h2 {
  animation: fadeIn 0.6s ease-in-out;
  font-size: 1.4rem;
  margin-top: 1rem;
  color: #0f5132;
}

.score {
  margin-top: 2rem;
  font-size: 1.2rem;
  color: #444;
  background: #f0f4fa;
  padding: 0.8rem 1.5rem;
  display: inline-block;
  border-radius: 10px;
  box-shadow: inset 0 0 3px rgba(0, 0, 0, 0.05);
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>
