<template>
  <div>
    <h1>Nomira 🎯</h1>

    <AttachmentUsers @liste-validee="setUtilisateurs" />

    <div v-if="utilisateurs.length > 0">
      <ListUsers :utilisateurs="utilisateurs" />

      <button @click="lancerAnimation" :disabled="enCours">🎲 Tirer au sort</button>
      <button @click="resetListe" style="margin-left: 10px;">Réinitialiser</button>

      <div v-if="enCours" class="animation-box">
        🎡 En cours... <br />
        {{ nomAnime }}
      </div>

      <p v-else-if="nomTire" class="nom-final">
        ✅ Nom tiré : {{ nomTire }}
      </p>
    </div>

    <div v-else>
      <p>La liste est vide.</p>
    </div>

    <div v-if="nomsTires.length > 0" style="margin-top: 20px;">
      <h2>Noms déjà tirés :</h2>
      <ul>
        <li v-for="(n, i) in nomsTires" :key="i">{{ n }}</li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import AttachmentUsers from './components/AttachmentUsers.vue'
import ListUsers from './components/ListUsers.vue'

const utilisateurs = ref([])
const nomTire = ref(null)
const nomsTires = ref([])
const nomAnime = ref('')
const enCours = ref(false)
let interval = null

onMounted(() => {
  const saved = localStorage.getItem('nomira_utilisateurs')
  if (saved) utilisateurs.value = JSON.parse(saved)

  const savedTires = localStorage.getItem('nomira_tires')
  if (savedTires) nomsTires.value = JSON.parse(savedTires)
})

function setUtilisateurs(liste) {
  utilisateurs.value = liste
  nomsTires.value = []
  nomTire.value = null
  localStorage.setItem('nomira_utilisateurs', JSON.stringify(liste))
  localStorage.removeItem('nomira_tires')
}

function resetListe() {
  utilisateurs.value = []
  nomsTires.value = []
  nomTire.value = null
  nomAnime.value = ''
  enCours.value = false
  clearInterval(interval)
  localStorage.removeItem('nomira_utilisateurs')
  localStorage.removeItem('nomira_tires')
}

function lancerAnimation() {
  if (utilisateurs.value.length === 0) return

  enCours.value = true
  nomTire.value = ''
  nomAnime.value = ''

  interval = setInterval(() => {
    const index = Math.floor(Math.random() * utilisateurs.value.length)
    nomAnime.value = utilisateurs.value[index]
  }, 75)

  setTimeout(() => {
    clearInterval(interval)

    const indexFinal = Math.floor(Math.random() * utilisateurs.value.length)
    const choisi = utilisateurs.value.splice(indexFinal, 1)[0]

    nomTire.value = choisi
    nomsTires.value.push(choisi)
    nomAnime.value = ''
    enCours.value = false

    localStorage.setItem('nomira_utilisateurs', JSON.stringify(utilisateurs.value))
    localStorage.setItem('nomira_tires', JSON.stringify(nomsTires.value))
  }, 2000)
}
</script>

<style scoped>
.app-container {
  display: flex;
  flex-wrap: wrap;
  gap: 40px;
  padding: 20px;
  justify-content: center;
  align-items: flex-start;
}

.left-panel, .right-panel {
  flex: 1 1 400px;
  max-width: 600px;
}

.animation-box {
  font-size: 2.2rem;
  font-weight: bold;
  padding: 40px;
  border: 5px solid;
  border-radius: 16px;
  background-color: #fff8dc;
  text-align: center;
  animation: colorChange 2s infinite alternate, pulse 0.6s infinite alternate;
  min-height: 150px;
}

.nom-final {
  font-size: 2.5rem;
  font-weight: bold;
  text-align: center;
  margin-top: 20px;
  padding: 20px;
  border-radius: 8px;
  background-color: #e6f7ff;
  color: #005a9c;
}

@keyframes pulse {
  from {
    transform: scale(1);
  }
  to {
    transform: scale(1.05);
  }
}

@keyframes colorChange {
  0% {
    border-color: #ff9800;
    background-color: #fff3e0;
  }
  50% {
    border-color: #4caf50;
    background-color: #e8f5e9;
  }
  100% {
    border-color: #2196f3;
    background-color: #e3f2fd;
  }
}
</style>
