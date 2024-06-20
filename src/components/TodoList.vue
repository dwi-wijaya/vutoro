<template>
    <div class="todo-list max-w-lg w-full mx-auto mt-10 py-6 px-8 bg-white  rounded-xl border">
        <h1 class="text-2xl font-bold mb-6 text-center">Vue To-Do List</h1>
        <div class="flex gap-3 items-center mb-8 ">
            <input v-model="newTodo" @keyup.enter="addTodo" placeholder="What do you need to do?"
                class="w-full p-2 rounded-md shadow-[5px_5px_0px_0px] outline outline-1 mb-1 btn-base relative px-3" />
            <button @click="addTodo" class="btn bg-green-200"><i class="fa fa-plus "></i></button>
        </div>
        <div>
            <ul>
                <li v-for="todo in incompleteTodos" :key="todo.id" class="flex items-center mb-4 gap-4 todo-item">
                    <div class="flex items-center flex-1 ">
                        <input type="checkbox" v-model="todo.completed" class="mr-2 w-4 h-4 border" @change="saveTodos" />
                        <span>{{ todo.text }}</span>
                    </div>
                    <button @click="removeTodo(todo.id)" class="">
                        <i class="fad fa-trash text-sm text-red-600"></i>
                    </button>
                </li>
            </ul>
        </div>
        <div class="">
            <h2 class="text-xl font-semibold my-6 leading-4 text-center">Completed Tasks</h2>
            <ul>
                <li v-for="todo in completedTodos" :key="todo.id" class="flex items-center justify-between mb-4 todo-item">
                    <div class="flex items-center">
                        <input type="checkbox" v-model="todo.completed" class="mr-2 accent-green-600 w-4 h-4 border"
                            @change="saveTodos" />
                        <span class="line-through text-slate-600">{{ todo.text }}</span>
                    </div>
                    <button @click="removeTodo(todo.id)" class="">
                        <i class="fad fa-trash text-sm"></i>
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
  
