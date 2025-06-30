<template>
    <div>
        <h2>Saisie des utilisateurs</h2>
        <input v-model="nom" placeholder="Entrez un nom" @keyup.enter="addName" />
        <button @click="addName">Ajouter</button>
        <ul>
            <li v-for="(n, i) in noms" :key="i">{{ n }}</li>
        </ul>
        <button @click="valListe" :disabled="noms.length === 0">Valider la liste</button>
    </div>
</template>

<script setup>
import { ref } from 'vue';

const nom = ref('');
const noms = ref([]);

const emit = defineEmits(['liste-validee']);

function addName() {
    if (nom.value.trim() !== '') {
        noms.value.push(nom.value.trim());
        nom.value = '';
    }
}

function valListe() {
    emit('liste-validee', noms.value);
    noms.value = [];

}
</script>

<style scoped>
input {
    width: 70%;
    margin-bottom: 10px;
}

ul {
    margin: 10px 0;
    padding-left: 20px;
}

li {
    list-style: disc;
}
</style>