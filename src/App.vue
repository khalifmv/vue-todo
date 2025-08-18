<script setup lang="ts">
import Header from './components/Header.vue'
import Button from './components/Button.vue'
import TodoCard from './components/TodoCard.vue'
import Dialog from './components/Dialog.vue'
import {ref, watch, onMounted} from 'vue'

const todos = ref([])
const newTodoText = ref('')
const editTodoText = ref('')
const editTodoId = ref()
const toggleEdit = ref(false)

onMounted(()=>{
  const todosFromLocal = JSON.parse(localStorage.getItem('todos')) ?? []
  if(todosFromLocal.length > 0){
    todos.value = todosFromLocal
  }
})

watch(todos, async () => {
  console.log(todos)
  localStorage.setItem('todos', JSON.stringify(todos.value))
}, {deep: true})

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

const editTodo = (id: number) => {
  toggleEdit.value = true
  editTodoId.value = id
  editTodoText.value = todos.value.find((x)=>x.id === id).text ?? ''
}

const confirmEditedTodo = () => {
  const index = todos.value.findIndex((x)=>x.id===editTodoId.value)
  todos.value[index].text = editTodoText
  toggleEdit.value = false
}
</script>

<template>
  <div class="max-w-[460px] mx-auto mt-10 px-2 md:px-0">
    <Header />
    <Dialog :title="'Edit'" :open="toggleEdit" @close="(e) => toggleEdit = false">
      <div class="sticky top-0 pb-3 pt-2 bg-white mt-4 flex gap-2 justify-center items-center">
        <input v-model="editTodoText" @keyup.enter="confirmEditedTodo" type="text" class="border border-t-0 border-x-0 border-gray-300 border-1 px-3 py-2 w-full" placeholder="Edit Task..." />
        <Button @click="confirmEditedTodo">
          <img src="/check.svg" alt="Add" class="w-6 h-6 invert" />
        </Button>
      </div>
    </Dialog>
    <div class="sticky top-0 pb-3 pt-2 bg-white mt-4 flex gap-2 justify-center items-center">
      <input v-model="newTodoText" @keyup.enter="addTodo" type="text" class="border border-t-0 border-x-0 border-gray-300 border-1 px-3 py-2 w-full" placeholder="Add a new task..." />
      <Button @click="addTodo">
        <img src="/plus.svg" alt="Add" class="w-6 h-6 invert" />
      </Button>
    </div>
    <div v-if="todos.length > 0" class="mt-2 gap-2 flex flex-col">
      <template v-for="todo in todos" :key="todo.id">
        <TodoCard 
          :todo="todo" 
          @toggle="(id, val) => toggleTodo(id, val)" 
          @delete="(id) => deleteTodo(id)" 
          @edit="(id) => editTodo(id)" 
        />
      </template>
    </div>
    <div v-else class="flex justify-center items-center min-h-[100px]">
      <p>You don't have any tasks yet.</p>
    </div>
    <p v-if="todos.length > 0" class="my-4">Your remaining todos: {{ todos.filter(todo => !todo.completed).length }}</p>
  </div>
</template>

<style scoped></style>
