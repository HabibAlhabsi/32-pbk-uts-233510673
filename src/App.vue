<script setup>
import { ref } from 'vue'

const tasks = ref([])
const inputTask = ref('')

const addTask = () => {
  if (inputTask.value.trim() !== '') {
    tasks.value.push({
      id: Date.now(),
      task: inputTask.value,
      completed: false
    })
    inputTask.value = ''
  }
}

const removeTask = (taskId) => {
  tasks.value = tasks.value.filter(task => task.id !== taskId)
}

</script>

<template>
  <div>
    <input type="text" v-model="inputTask" @keyup.enter="addTask" />
    <button @click="addTask">Add Task</button>

    <ul>
      <li v-for="task in tasks" :key="task.id">
        <input type="checkbox" v-model="task.completed" />
        {{ task.task }}
        <button @click="removeTask(task.id)">Remove</button>
      </li>
    </ul>
  </div>
</template>

<style scoped></style>
