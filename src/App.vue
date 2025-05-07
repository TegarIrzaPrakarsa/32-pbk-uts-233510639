<template>
  <div class="app">
    <h1>📝 Daftar Kegiatan</h1>

    <div class="input-section">
      <input v-model="newTask" @keyup.enter="addTask" placeholder="Tambahkan kegiatan..." />
      <button @click="addTask">Tambah</button>
    </div>

    <div class="filter">
      <label>
        <input type="checkbox" v-model="showOnlyIncomplete" />
        Tampilkan hanya yang belum selesai
      </label>
    </div>

    <ul class="task-list">
      <li
        v-for="(task, index) in filteredTasks"
        :key="index"
        :class="{ done: task.completed }"
      >
        <input type="checkbox" v-model="task.completed" />
        <span>{{ task.text }}</span>
        <button @click="removeTask(index)">❌</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const tasks = ref([])
const newTask = ref('')
const showOnlyIncomplete = ref(false)

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push({ text: newTask.value, completed: false })
    newTask.value = ''
  }
}

const removeTask = (index) => {
  tasks.value.splice(index, 1)
}

const filteredTasks = computed(() =>
  showOnlyIncomplete.value
    ? tasks.value.filter((task) => !task.completed)
    : tasks.value
)
</script>

<style scoped>
.app {
  max-width: 500px;
  margin: 40px auto;
  padding: 20px;
  background: #f7f9fc;
  border-radius: 12px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  margin-bottom: 20px;
}

.input-section {
  display: flex;
  gap: 10px;
}

input[type='text'] {
  flex: 1;
  padding: 8px;
  border-radius: 6px;
  border: 1px solid #ccc;
}

button {
  background-color: #42b883;
  border: none;
  color: white;
  padding: 8px 12px;
  border-radius: 6px;
  cursor: pointer;
  transition: background 0.2s;
}

button:hover {
  background-color: #2c8f6c;
}

ul.task-list {
  list-style: none;
  padding: 0;
  margin-top: 20px;
}

li {
  background: white;
  margin-bottom: 10px;
  padding: 10px;
  border-radius: 6px;
  display: flex;
  align-items: center;
  gap: 10px;
}

li.done span {
  text-decoration: line-through;
  color: gray;
}

.filter {
  margin-top: 15px;
  font-size: 0.9rem;
}
</style>
