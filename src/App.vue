<script setup lang="ts">
import Header from './components/Header.vue'
import Button from './components/Button.vue'
import TodoCard from './components/TodoCard.vue'
import {ref} from 'vue'

const todos = ref([
  { id: 1, text: 'Todo Item 1', completed: false },
  { id: 2, text: 'Todo Item 2', completed: false },
  { id: 3, text: 'Todo Item 3', completed: true }
])

const newTodoText = ref('')

const toggleTodo = (id: number, val: Boolean) => {
  
  const todo = todos.value.find(todo => todo.id === id)
  if (todo) {
    todo.completed = val
  }
}
const deleteTodo = (id: number) => {
  todos.value = todos.value.filter(todo => todo.id !== id)
}

const addTodo = () => {
  if (newTodoText.value.trim() !== '') {
    todos.value.push({ id: todos.value.length + 1, text: newTodoText.value, completed: false })
    newTodoText.value = ''
  }
}
</script>

<template>
  <div class="max-w-[460px] mx-auto mt-10">
    <Header />
    <div class="mt-4 flex gap-2 justify-center items-center">
      <input v-model="newTodoText" @keyup.enter="addTodo" type="text" class="border border-t-0 border-x-0 border-gray-300 border-1 px-3 py-2 w-full" placeholder="Add a new task..." />
      <Button @click="addTodo">
        <img src="/plus.svg" alt="Add" class="w-6 h-6 invert" />
      </Button>
    </div>
    <div class="mt-2 gap-2 flex flex-col">
      <template v-for="todo in todos" :key="todo.id">
        <TodoCard :todo="todo" @toggle="(id, val) => toggleTodo(id, val)" @delete="(id) => deleteTodo(id)" />
      </template>
    </div>
    <p class="my-4">Your remaining todos: {{ todos.filter(todo => !todo.completed).length }}</p>
  </div>
</template>

<style scoped></style>
