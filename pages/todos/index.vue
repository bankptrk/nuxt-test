<script setup>
import { ref } from 'vue'

const BASE_URL = 'https://66fe6cbc2b9aac9c997be2f2.mockapi.io'

const todos = ref([])
const todoText = ref('')

const fetchTodo = async () => {
    const { data, error } = await useFetch(`${BASE_URL}/todos`)
    if (!error.value) {
        todos.value = data.value
    } else {
        console.log('Error fetching todos:', error.value)
    }
}

const addTodo = async () => {
    try {
        const { error } = await useFetch(`${BASE_URL}/todos`, {
            method: 'POST',
            body: {
                name: todoText.value,
                status: 'pending'
            }
        })
        if (error.value) {
            console.log('Error adding todo:', error.value)
        } else {
            await fetchTodo()
            todoText.value = ''
        }
    } catch (error) {
        console.log('Unexpected error:', error)
    }
}

const todoEdit = async (todoId, todoData) => {
    try {
        const { error } = await useFetch(`${BASE_URL}/todos/${todoId}`, {
            method: 'PUT',
            body: {
                status: todoData.status
            }
        })
        if (error.value) {
            console.log('Error updating todo:', error.value)
        } else {
            await fetchTodo()
        }
    } catch (error) {
        console.log('Unexpected error:', error)
    }
}

const deleteTodo = async (todoId) => {
    try {
        const { error } = await useFetch(`${BASE_URL}/todos/${todoId}`, {
            method: 'DELETE'
        })
        if (error.value) {
            console.log('Error delete todo:', error.value)
        } else {
            await fetchTodo()
        }
    } catch (error) {
        console.log('error', error)
    }
}

fetchTodo()
</script>

<template>
    <div>
        <input v-model="todoText" type="text" placeholder="Enter todo">
        <button @click="addTodo">Add</button>
    </div>
    <ul>
        <li v-for="todo in todos" :key="todo.id">
            {{ todo.id }} - {{ todo.name }} - {{ todo.status }}
            <select v-model="todo.status">
                <option>Pending</option>
                <option>Doing</option>
                <option>Done</option>
            </select>
            <button @click="todoEdit(todo.id, todo)">Update</button>
            <NuxtLink :to="`/todos/${todo.id}`">Edit</NuxtLink>
            <button @click="deleteTodo(todo.id)">Delete</button>
        </li>
    </ul>
</template>
