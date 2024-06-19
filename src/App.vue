<template>
  <div id="app" class="bg-gray-100 min-h-screen flex items-center justify-center">
    <TodoList />
  </div>
</template>

<script setup>
import TodoList from './components/TodoList.vue'
</script>

<style>
body {
  @apply bg-gray-100;
}
</style>
