<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('')

const input_content = ref('')
const input_category = ref(null)

const todos_asc = computed(() =>
  todos.value.sort((a, b) => {
    return b.createdAt - a.createdAt
  })
)

const addTodo = () => {
  if(input_content.value.trim() === '' || input_category.value === null) {
    return
  }
  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime()
  })
}

watch(todos, newVal => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, { deep: true })

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
})
</script>

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
            <input type="radio" name="category" value="Harian" v-model="input_category" />
            <span class="bubble business"></span>
            <div>Harian</div>
          </label>
          <label>
            <input type="radio" name="category" value="Tugas" v-model="input_category" />
            <span class="bubble personal"></span>
            <div>Tugas</div>
          </label>
        </div>
        <input type="submit" value="Add Todo" />
      </form>
    </section>
  </main>
</template>
