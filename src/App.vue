<script setup>
import { ref, computed } from 'vue'

const tasks = ref([])
const inputTask = ref('')
const filter = ref('all')

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

const filteredTasks = computed(() => {
  if (filter.value === 'all') {
    return tasks.value
  } else if (filter.value === 'completed') {
    return tasks.value.filter(task => task.completed)
  } else {
    return tasks.value.filter(task => !task.completed)
  }
})

const removeTask = (taskId) => {
  tasks.value = tasks.value.filter(task => task.id !== taskId)
}

</script>

<template>
  <div>
    <input type="text" v-model="inputTask" @keyup.enter="addTask" />
    <button @click="addTask">Add Task</button>
    <select v-model="filter">
      <option value="all">All</option>
      <option value="completed">Completed</option>
      <option value="incomplete">Incomplete</option>
    </select>

    <ul>
      <li v-for="task in filteredTasks" :key="task.id">
        <input type="checkbox" v-model="task.completed" />
        {{ task.task }}
        <button @click="removeTask(task.id)">Remove</button>
      </li>
    </ul>
  </div>
</template>

<style scoped></style>
