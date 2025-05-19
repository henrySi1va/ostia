<template>
  <div>
    <h1>Task Tracker</h1>
    <form @submit.prevent="addTask">
      <input v-model="newTask" placeholder="New Task" />
      <button type="submit">Add</button>
    </form>
    <ul>
      <li v-for="task in tasks" :key="task._id">
        {{ task.title }} - {{ task.completed ? '✅' : '❌' }}
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const newTask = ref('');
const tasks = ref([]);

async function fetchTasks() {
  const res = await axios.get('http://localhost:5000/tasks');
  tasks.value = res.data;
}

async function addTask() {
  await axios.post('http://localhost:5000/tasks', {
    title: newTask.value,
    completed: false,
  });
  newTask.value = '';
  fetchTasks();
}

onMounted(fetchTasks);
</script>

<style>
body {
  font-family: sans-serif;
  margin: 2rem;
}
</style>
