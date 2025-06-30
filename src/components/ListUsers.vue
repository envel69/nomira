<template>
    <div v-if="utilisateurs.length > 0">
        <h2>Liste des utilisateurs :</h2>
        <ul>
            <li v-for="(nom, index) in utilisateurs" :key="index">{{ nom }}</li>
        </ul>
    </div>
</template>

<script setup>
defineProps({
    utilisateurs: Array
})
</script>