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
</script>

<template>
  <div>
    <input type="text" v-model="inputTask" @keyup.enter="addTask" />
    <button @click="addTask">Add Task</button>

    <ul>
      <li v-for="task in tasks" :key="task.id">
        {{  task.task }}
      </li>
    </ul>
  </div>
</template>

<style scoped>

</style>
