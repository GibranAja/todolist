<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">Halo, <input type="text" placeholder="Nama Kamu" v-model="name" /></h2>
    </section>
    <section class="create-todo">
      <h3>BUAT TODO KAMU</h3>
      <form @submit.prevent="addTodo">
        <h4>Apa yang ada didalam todo list kamu</h4>
        <input type="text" placeholder="contoh: mau makan siang" v-model="input_content" />
        <h4>Pilih Kategori</h4>
        <div class="options">
          <label>
            <input type="radio" name="category" value="business" v-model="input_category" />
            <span class="bubble business"></span>
            <div>Harian</div>
          </label>
          <label>
            <input type="radio" name="category" value="personal" v-model="input_category" />
            <span class="bubble personal"></span>
            <div>Tugas</div>
          </label>
        </div>
        <input type="submit" value="Add Todo" />
      </form>
    </section>

    <section class="todo-list">
      <h3>TODO LIST</h3>
      <div v-if="todosAsc.length > 0" class="list">
        <div v-for="todo in todosAsc" :class="`todo-item ${todo.done && 'done'}`">
          <label>
            <input type="checkbox" v-model="todo.done" />
            <span :class="`bubble ${todo.category}`"></span>
          </label>
          <div class="todo-content">
            <input type="text" v-model="todo.content" />
          </div>
          <div class="actions">
            <button class="delete" @click="removeTodo(todo)">Hapus</button>
          </div>
        </div>
      </div>
      <p v-else class="empty-todo-message">Kamu belum membuat todo apapun nih -_-</p>
    </section>
  </main>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'

const todos = ref([])
const name = ref('')

const input_content = ref('')
const input_category = ref(null)

const todosAsc = () =>
  todos.value.sort((a, b) => {
    return b.createdAt - a.createdAt
  })

const addTodo = () => {
  if (input_content.value.trim() === '' || input_category.value === null) {
    return
  }
  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime()
  })

  input_category.value = ''
  input_content.value = null
}

const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo)
}

watch(
  todos,
  (newVal) => {
    localStorage.setItem('todos', JSON.stringify(newVal))
  },
  { deep: true }
)

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})
</script>

<style scoped>
.empty-todo-message {
  margin-top: 1.5rem;
  text-align: center;
  color: var(--grey);
  font-style: italic;
}
</style>
