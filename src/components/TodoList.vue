<template>
    <div class="todo-list max-w-md mx-auto mt-10 p-4 bg-white shadow-lg rounded-lg">
        <h1 class="text-2xl font-bold mb-4">Vue To-Do List</h1>
        <input v-model="newTodo" @keyup.enter="addTodo" placeholder="Add a new task"
            class="w-full p-2 mb-4 border rounded" />
        <ul>
            <li v-for="(todo, index) in todos" :key="index" class="flex items-center justify-between mb-2 gap-5">
                <div class="flex items-center">
                    <input type="checkbox" v-model="todo.completed" class="mr-2" @change="saveTodos" />
                    <span :class="{ 'line-through': todo.completed }">{{ todo.text }}</span>
                </div>
                <button @click="removeTodo(index)" class="bg-red-500 text-white p-2 rounded">
                    Delete
                </button>
            </li>
        </ul>
    </div>
</template>
  
<script setup>
import { ref, onMounted, watch } from 'vue'

const newTodo = ref('')
const todos = ref([])

const addTodo = () => {
    if (newTodo.value.trim()) {
        todos.value.push({
            text: newTodo.value,
            completed: false,
        })
        newTodo.value = ''
        saveTodos()
    }
}

const removeTodo = (index) => {
    todos.value.splice(index, 1)
    saveTodos()
}

const saveTodos = () => {
    localStorage.setItem('todos', JSON.stringify(todos.value))
}

onMounted(() => {
    const savedTodos = localStorage.getItem('todos')
    if (savedTodos) {
        todos.value = JSON.parse(savedTodos)
    } else {
        // Initial data
        todos.value = [
            { text: 'Learn Vue 3', completed: false },
            { text: 'Build a To-Do app', completed: false },
        ]
        saveTodos()
    }
})

watch(todos, saveTodos, { deep: true })
</script>
  
<style scoped>
.line-through {
    text-decoration: line-through;
}
</style>
  