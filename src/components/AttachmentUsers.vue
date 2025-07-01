<template>
    <div>
        <h2>Entrer un nom:</h2>
        <input v-model="name" placeholder="Enter a name" @keyup.enter="addName" />
        <button @click="addName">Ajouter</button>
        <ul>
            <li v-for="(n, i) in names" :key="i">
                <template v-if="editIndex === i">
                    <input v-model="editName" @keyup.enter="saveEdit(i)" @blur="saveEdit(i)" style="width: 60%;" />
                    <button @click="saveEdit(i)">💾</button>
                    <button @click="cancelEdit">✖️</button>
                </template>
                <template v-else>
                    {{ n }}
                    <button @click="startEdit(i, n)" title="Edit">✏️</button>
                    <button @click="removeName(i)" title="Delete">🗑️</button>
                </template>
            </li>
        </ul>
        <button @click="validateList" :disabled="names.length === 0">Valider les noms</button>
    </div>
</template>

<script setup>
import { ref } from 'vue';

const name = ref('');
const names = ref([]);

const editIndex = ref(null);
const editName = ref('');

const emit = defineEmits(['list-validated']);

function addName() {
    if (name.value.trim() !== '') {
        names.value.push(name.value.trim());
        name.value = '';
    }
}

function removeName(index) {
    names.value.splice(index, 1);
}

function startEdit(index, value) {
    editIndex.value = index;
    editName.value = value;
}

function saveEdit(index) {
    if (editName.value.trim() !== '') {
        names.value[index] = editName.value.trim();
    }
    editIndex.value = null;
    editName.value = '';
}

function cancelEdit() {
    editIndex.value = null;
    editName.value = '';
}

function validateList() {
    emit('list-validated', names.value);
    names.value = [];
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