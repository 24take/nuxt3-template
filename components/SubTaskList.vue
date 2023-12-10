<script setup lang="ts">
import { ref } from 'vue'
import { computed } from 'vue'

interface SubTask {
  name: string
  done: boolean
}

const newSubTaskName = ref('')

const subTasks = ref<SubTask[]>([
])

const addSubTask = () => {
  if (newSubTaskName.value !== '') subTasks.value.push({ name: newSubTaskName.value, done: false })
  newSubTaskName.value = ''
}

const notFinishedSubTasks = computed(() =>
  subTasks.value.filter((subTask) => !subTask.done)
)
</script>

<template>
  <label>
    <input v-model="newSubTaskName" type="text" />
  </label>
  <button @click="addSubTask">サブタスクを追加</button>
  <ul>
    <li v-for="subTask in notFinishedSubTasks" :key="subTask.name">
      <div>サブタスク名: {{ subTask.name }}</div>
      <button @click="subTask.done = true">完了</button>
    </li>
  </ul>
</template>
