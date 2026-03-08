<template>
  <div>
    <h1>Todo App</h1>
    <input v-model="newTask" @keyup.enter="addTasks" placeholder="新しいタスク">
    <button @click="addTasks">追加</button>
    <ul>
      <li v-for="task in tasks" :key="task.id">
        {{ task.title }}
      <button @click="deleteTask(task.id!)">削除</button>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue';
import type { Task } from './types'
import axios from 'axios';

const tasks = ref<Task[]>([])
const newTask = ref<String>('')

async function fetchTasks() {
  const res = await axios.get('http://localhost:8080/api/tasks')
  tasks.value = res.data
}

async function addTasks() {
  if (!newTask.value.trim()) return
  await axios.post('http://localhost:8080/api/tasks', { title: newTask.value })
  newTask.value = ''
  await fetchTasks()
}

async function deleteTask(id: number) {
  await axios.delete(`http://localhost:8080/api/tasks/${id}`)
  await fetchTasks()

}
onMounted(fetchTasks())

</script>