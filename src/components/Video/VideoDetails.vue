<script setup>
 const props = defineProps(["title", "username"]);
</script>

<template>
    <div>
        <h3>{{username}}</h3>
        <p>{{title}}</p>
    </div>
</template>

<style scoped>
    h3, p{
        font-family: Arial, Helvetica, sans-serif;
    }
</style>