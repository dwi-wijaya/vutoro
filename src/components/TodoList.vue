<template>
    <div class="todo-list max-w-md mx-auto mt-10 p-4 bg-white shadow-lg rounded-lg">
        <h1 class="text-2xl font-bold mb-4">Vue To-Do List</h1>
        <input v-model="newTodo" @keyup.enter="addTodo" placeholder="Add a new task"
            class="w-full p-2 mb-4 border rounded" />
        <div>
            <h2 class="text-xl font-semibold mb-2">Incomplete Tasks</h2>
            <ul>
                <li v-for="todo in incompleteTodos" :key="todo.id" class="flex items-center justify-between mb-2">
                    <div class="flex items-center">
                        <input type="checkbox" v-model="todo.completed" class="mr-2" @change="saveTodos" />
                        <span>{{ todo.text }}</span>
                    </div>
                    <button @click="removeTodo(todo.id)" class="bg-red-500 text-white p-2 rounded">
                        Delete
                    </button>
                </li>
            </ul>
        </div>
        <div class="mt-4">
            <h2 class="text-xl font-semibold mb-2">Completed Tasks</h2>
            <ul>
                <li v-for="todo in completedTodos" :key="todo.id" class="flex items-center justify-between mb-2">
                    <div class="flex items-center">
                        <input type="checkbox" v-model="todo.completed" class="mr-2" @change="saveTodos" />
                        <span class="line-through">{{ todo.text }}</span>
                    </div>
                    <button @click="removeTodo(todo.id)" class="bg-red-500 text-white p-2 rounded">
                        Delete
                    </button>
                </li>
            </ul>
        </div>
    </div>
</template>
  
<script setup>
import { ref, onMounted, watch, computed } from 'vue'
import { v4 as uuidv4 } from 'uuid'

const newTodo = ref('')
const todos = ref([])

const addTodo = () => {
    if (newTodo.value.trim()) {
        todos.value.push({
            id: uuidv4(),
            text: newTodo.value,
            completed: false,
        })
        newTodo.value = ''
        saveTodos()
    }
}

const removeTodo = (id) => {
    todos.value = todos.value.filter(todo => todo.id !== id)
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
            { id: uuidv4(), text: 'Learn Vue 3', completed: false },
            { id: uuidv4(), text: 'Build a To-Do app', completed: false },
        ]
        saveTodos()
    }
})

watch(todos, saveTodos, { deep: true })

const incompleteTodos = computed(() => todos.value.filter(todo => !todo.completed))
const completedTodos = computed(() => todos.value.filter(todo => todo.completed))
</script>
  
