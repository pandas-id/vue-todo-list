<script setup>
import { ref, computed } from 'vue'


let id = 0

// const counter = ref(0)

const newTodo = ref('')
const hideComplted = ref(false)

const isUpdate = ref(false)
const todoSelected = ref(null)

const todos = ref([
  { id: id++, text: 'Belajar Javascript', done: true },
  { id: id++, text: 'Belajar Vue', done: false },
  { id: id++, text: 'Belajar Python', done: true },
  { id: id++, text: 'Belajar React', done: false }
])

function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value, done: false })
  newTodo.value = ''
}

function editTodo(todo) {
  isUpdate.value = true
  newTodo.value = todo.text
  todoSelected.value = todo
}

function updateTodo() {
  todos.value.forEach((todo) => {
    if(todo.id == todoSelected.value.id ) {
      todo.text = newTodo.value
    }
  })
  // todos.value.map((t) => {
  //   if(t.id == todoSelected.value.id) {
  //     t.text = newTodo.value
  //   }
  // })
  newTodo.value = ''
  todoSelected.value = null
  isUpdate.value = false
}

function deleteTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo)
}

const filteredTodos = computed(() => {
  return hideComplted.value
    ? todos.value.filter((t) => !t.done)
    : todos.value
})

// function test() {
//   alert('hai')
// }

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
        <button class="button is-link is-light is-fullwidth" type="submit">Update Todo</button>
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
        <button class="button is-primary is-fullwidth" type="submit">Tambahkan</button>
      </div>
    </div>
  </form>
  <br>
  <h4 class="is-size-6 has-text-weight-bold">Todo Lists</h4>
  <hr>

  <div class="block">
    <div class="box is-size7 has-text-weight-bold" v-for="todo in filteredTodos" :key="todo.id">
      <input class="mr-3" type="checkbox" v-model="todo.done">
      <span class="" :class="{done: todo.done}">{{ todo.text }}</span>
      <div class="buttons are-small">
        <button class="button is-danger is-light" type="button" @click="deleteTodo(todo)">Hapus</button>
        <button class="button is-success is-light" type="button" @click="editTodo(todo)">Edit</button>
      </div>
    </div>
  </div>
  <button class="button is-warning is-light is-fullwidth" type="button" @click="hideComplted = !hideComplted">
    {{ hideComplted ? 'Tampilkan Semua' : 'Sembuyikan' }}
  </button>
</template>


<style>
.done {
  text-decoration: line-through;
}
</style>
