<script setup>
import { ref, computed } from 'vue'

let id = 0

const newTodo = ref('')
const hideCompleted = ref(false)

const isUpdate = ref(false)
const selectedTodo = ref(null)

const todos = ref([
  { id: id++, text: 'Belajar Javascript', done: true},
  { id: id++, text: 'Belajar Vue', done: false},
  { id: id++, text: 'Belajar Python', done: true},
])

const filteredTodos = computed(() => {
  return hideCompleted.value
    ? todos.value.filter((t) => !t.done)
    : todos.value
})

function addTodo() {
  todos.value.push({id: id++, text: newTodo.value, done: false})
  newTodo.value = ''
}

function deleteTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo)
}

function editTodo(todo) {
  isUpdate.value = true
  newTodo.value = todo.text
  selectedTodo.value = todo
}

function updateTodo() {
  todos.value.forEach((todo) => {
    if(todo === selectedTodo.value) todo.text = newTodo.value
  })
  newTodo.value = ''
  selectedTodo.value = null
}

</script>

<template>
  <h1 class="title has-text-centered">Todo App</h1>
  <form v-if="isUpdate" @submit.prevent="updateTodo">
    <div class="field">
      <label class="label">Update Todo</label>
      <div class="control">
        <input class="input" type="text" v-model="newTodo">
      </div>
    </div>
    <div class="field">
      <div class="control">
        <button class="button is-primary is-fullwidth" type="submit">Update</button>
      </div>
    </div>
  </form>
  <form v-else @submit.prevent="addTodo">
    <div class="field">
      <label class="label">Add Todo</label>
      <div class="control">
        <input class="input" type="text" v-model="newTodo">
      </div>
    </div>
    <div class="field">
      <div class="control">
        <button class="button is-primary is-fullwidth" type="submit">Add</button>
      </div>
    </div>
  </form>

  <h3 class="mt-6 is-size-6 has-text-weight-bold">Todo Lists</h3>
  <hr class="mt-2">
  <div class="block">
    <div v-for="todo in filteredTodos" :key="todo.id" class="box">
      <input class="mr-3" type="checkbox" v-model="todo.done">
      <span :class="{done: todo.done}">{{ todo.text }}</span>
      <div class="buttons are-small">
        <button @click="deleteTodo(todo)" class="button is-danger is-light" type="button">Delete</button>
        <button @click="editTodo(todo)" class="button is-primary is-light" type="button">Update</button>
      </div>
    </div>
  </div>

  <div class="block">
    <button @click="hideCompleted = !hideCompleted" class="button is-warning is-fullwidth is-light" type="button">{{ hideCompleted ? 'Show All' : 'Hide Completed' }}</button>
  </div>

</template>

<style>
.done {
  text-decoration: line-through;
}
</style>
