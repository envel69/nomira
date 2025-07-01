<template>
    <div>
      <h1>Nomira</h1>
  
      <AttachmentUsers @liste-validee="setUsers" />
  
      <div v-if="utilisateurs.length > 0">
        <ListUsers :utilisateurs="utilisateurs" />
  
        <button @click="Animation" :disabled="enCours">🎲 Tirer au sort</button>
        <button @click="resetList" style="margin-left: 10px;">Réinitialiser</button>
  
        <div v-if="enCours" class="animation-box">
           En cours... <br />
          {{ nomAnime }}
        </div>
  
        <p v-else-if="nomTire" class="nom-final">
          Nom tiré : {{ nomTire }}
        </p>
      </div>
      
      <div v-else>
        <p>La liste est vide.</p>
      </div>
  
      <div v-if="nomsTires.length > 0" style="margin-top: 20px;">
        <h2>Noms déjà tirés :</h2>
        
          <li v-for="(n, i) in nomsTires" :key="i">{{ n }}</li>
        
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue'
  import AttachmentUsers from '../components/AttachmentUsers.vue'
  import ListUsers from '../components/ListUsers.vue'

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
  
  function setUsers(liste) {
    utilisateurs.value = liste
    nomsTires.value = []
    nomTire.value = null
    localStorage.setItem('nomira_utilisateurs', JSON.stringify(liste))
    localStorage.removeItem('nomira_tires')
  }
  
  function resetList() {
    utilisateurs.value = []
    nomsTires.value = []
    nomTire.value = null
    nomAnime.value = ''
    enCours.value = false
    clearInterval(interval)
    localStorage.removeItem('nomira_utilisateurs')
    localStorage.removeItem('nomira_tires')
  }
  
  function Animation() {
    if (utilisateurs.value.length === 0) return
  
    enCours.value = true
    nomTire.value = ''
    nomAnime.value = ''
  
    interval = setInterval(() => {
      const index = Math.floor(Math.random() * utilisateurs.value.length)
      nomAnime.value = utilisateurs.value[index]
    },)
  
    setTimeout(() => {
      clearInterval(interval)
  
      const i = Math.floor(Math.random() * utilisateurs.value.length)
      const choisi = utilisateurs.value.splice(i, 1)[0]
  
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
  .animation-box {
    margin-top: 20px;
    font-size: 2rem;
    font-weight: bold;
    padding: 20px;
    border: 3px dashed #4caf50;
    border-radius: 12px;
    background: #eaffea;
    text-align: center;
    animation: pulse 0.5s infinite alternate;
  }
  
  .nom-final {
    font-size: 2.5rem;
    font-weight: bold;
    margin-top: 20px;
    text-align: center;
    color: #2e7d32;
    transition: transform 0.3s ease-in-out;
  }
  
  @keyframes pulse {
    from {
      transform: scale(1);
      background-color: #eaffea;
    }
    to {
      transform: scale(1.05);
      background-color: #d1ffd1;
    }
  }
  </style>
  