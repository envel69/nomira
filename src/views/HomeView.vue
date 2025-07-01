<template>
    <div>
      <h1>Nomira</h1>
  
      <AttachmentUsers @list-validated="setUsers" />
  
      <div v-if="users.length > 0">
        <ListUsers :users="users" />
  
        <button @click="drawAnimation" :disabled="isDrawing">🎲 Tirage</button>
        <button @click="resetList" style="margin-left: 10px;">Réinitialiser</button>
  
        <div v-if="isDrawing" class="animation-box">
          Tirage... <br />
          {{ animatedName }}
        </div>
  
        <p v-else-if="drawnName" class="final-name">
          Nom tiré au sort: {{ drawnName }}
        </p>
      </div>
      
      <div v-else>
        <p>Il n'y a plus de nom à tirer.</p>
      </div>
  
      <div v-if="drawnNames.length > 0" style="margin-top: 20px;">
        <h2>Les noms des personnes tirées:</h2>
        <li v-for="(n, i) in drawnNames" :key="i">{{ n }}</li>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue'
  import AttachmentUsers from '../components/AttachmentUsers.vue'
  import ListUsers from '../components/ListUsers.vue'
  
  const users = ref([])
  const drawnName = ref(null)
  const drawnNames = ref([])
  const animatedName = ref('')
  const isDrawing = ref(false)
  let interval = null
  
  onMounted(() => {
    const saved = localStorage.getItem('nomira_users')
    if (saved) users.value = JSON.parse(saved)
  
    const savedDrawn = localStorage.getItem('nomira_drawn')
    if (savedDrawn) drawnNames.value = JSON.parse(savedDrawn)
  })
  
  function setUsers(list) {
    users.value = list
    drawnNames.value = []
    drawnName.value = null
    localStorage.setItem('nomira_users', JSON.stringify(list))
    localStorage.removeItem('nomira_drawn')
  }
  
  function resetList() {
    users.value = []
    drawnNames.value = []
    drawnName.value = null
    animatedName.value = ''
    isDrawing.value = false
    clearInterval(interval)
    localStorage.removeItem('nomira_users')
    localStorage.removeItem('nomira_drawn')
  }
  
  function drawAnimation() {
    if (users.value.length === 0) return
  
    isDrawing.value = true
    drawnName.value = ''
    animatedName.value = ''
  
    interval = setInterval(() => {
      const index = Math.floor(Math.random() * users.value.length)
      animatedName.value = users.value[index]
    }, )
  
    setTimeout(() => {
      clearInterval(interval)
  
      const i = Math.floor(Math.random() * users.value.length)
      const chosen = users.value.splice(i, 1)[0]
  
      drawnName.value = chosen
      drawnNames.value.push(chosen)
      animatedName.value = ''
      isDrawing.value = false
  
      localStorage.setItem('nomira_users', JSON.stringify(users.value))
      localStorage.setItem('nomira_drawn', JSON.stringify(drawnNames.value))
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
  
  .final-name {
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