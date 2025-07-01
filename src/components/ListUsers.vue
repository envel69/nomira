<template>
    <div v-if="users.length > 0">
        <h2>Liste des utilisateurs :</h2>
        <ul>
            <li v-for="(name, index) in users" :key="index">{{ name }}</li>
        </ul>
    </div>
</template>

<script setup>
defineProps({
    users: Array
})
</script>