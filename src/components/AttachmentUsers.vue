<template>
    <div>
        <h2>Entrez votre nom</h2>
        <input v-model="nom" placeholder="Entrez un nom" @keyup.enter="addName" />
        <button @click="addName">Ajouter</button>
        <ul>
            <li v-for="(n, i) in noms" :key="i">
                <template v-if="editIndex === i">
                    <input v-model="editNom" @keyup.enter="saveEdit(i)" @blur="saveEdit(i)" style="width: 60%;" />
                    <button @click="saveEdit(i)">💾</button>
                    <button @click="cancelEdit">✖️</button>
                </template>
                <template v-else>
                    {{ n }}
                    <button @click="startEdit(i, n)" title="Modifier">✏️</button>
                    <button @click="removeName(i)" title="Supprimer">🗑️</button>
                </template>
            </li>
        </ul>
        <button @click="valListe" :disabled="noms.length === 0">Valider la liste</button>
    </div>
</template>

<script setup>
import { ref } from 'vue';

const nom = ref('');
const noms = ref([]);

const editIndex = ref(null);
const editNom = ref('');

const emit = defineEmits(['liste-validee']);

function addName() {
    if (nom.value.trim() !== '') {
        noms.value.push(nom.value.trim());
        nom.value = '';
    }
}

function removeName(index) {
    noms.value.splice(index, 1);
}

function startEdit(index, value) {
    editIndex.value = index;
    editNom.value = value;
}

function saveEdit(index) {
    if (editNom.value.trim() !== '') {
        noms.value[index] = editNom.value.trim();
    }
    editIndex.value = null;
    editNom.value = '';
}

function cancelEdit() {
    editIndex.value = null;
    editNom.value = '';
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
    margin-bottom: 6px;
}
button {
    margin-left: 6px;
}
</style>