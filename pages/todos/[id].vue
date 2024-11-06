<script setup>
const route = useRoute()
const todoId = route.params.id
const BASE_URL = 'https://66fe6cbc2b9aac9c997be2f2.mockapi.io'


const { data: todo } = await useFetch(`${BASE_URL}/todos/${todoId}`)



</script>

<template>
    <div>
        {{ todoId }} - {{ todo.name }}
    </div>

</template>