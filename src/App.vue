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
  <div class="flex h-screen bg-gray-100">
    <!-- Sidebar -->
    <aside class="w-1/4 bg-gray-800 text-white p-6 shadow-md">
      <h2 class="text-2xl font-bold mb-6">Task Panel</h2>
      <div class="space-y-4">
        <label class="block font-medium">Filter:</label>
        <select
          v-model="filter"
          class="w-full px-3 py-2 rounded bg-gray-700 text-white border border-gray-600 focus:outline-none focus:ring-2 focus:ring-blue-500"
        >
          <option value="all">All</option>
          <option value="completed">Completed</option>
          <option value="incomplete">Incomplete</option>
        </select>
      </div>
    </aside>

    <!-- Main Content -->
    <main class="flex-1 p-8 overflow-y-auto">
      <h1 class="text-3xl font-bold text-gray-800 mb-6">My Tasks</h1>

      <div class="mb-6 flex gap-4">
        <input
          type="text"
          v-model="inputTask"
          @keyup.enter="addTask"
          placeholder="Enter new task"
          class="flex-1 px-4 py-2 rounded border border-gray-400 focus:outline-none focus:ring-2 focus:ring-blue-500"
        />
        <button
          @click="addTask"
          class="bg-blue-600 text-white px-6 py-2 rounded hover:bg-blue-700"
        >
          Add Task
        </button>
      </div>

      <ul class="space-y-4">
        <li
          v-for="task in filteredTasks"
          :key="task.id"
          class="flex items-center justify-between bg-white rounded shadow px-4 py-2"
        >
          <div class="flex items-center gap-3">
            <input
              type="checkbox"
              v-model="task.completed"
              class="accent-blue-600 w-5 h-5"
            />
            <span
              :class="{
                'line-through text-gray-500': task.completed,
                'text-gray-800': !task.completed
              }"
              class="text-lg"
            >
              {{ task.task }}
            </span>
          </div>
          <button
            @click="removeTask(task.id)"
            class="text-red-600 hover:text-red-800 transition"
            title="Remove"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-6 w-6"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
              stroke-width="2"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M6 18L18 6M6 6l12 12"
              />
            </svg>
          </button>
        </li>
      </ul>
    </main>
  </div>
</template>

<style scoped></style>
