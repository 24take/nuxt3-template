<script setup lang="ts">
import { ref } from 'vue'
import { computed } from 'vue'
import SubTaskList from './SubTaskList.vue'

interface SubTask {
  name: string
  done: boolean
}
interface Task {
  name: string
  done: boolean
  subTasks: SubTask[]
}

const tasks = ref<Task[]>([
  { name: 'A', done: false, subTasks: [] },
  { name: 'B', done: false, subTasks: [] }
])
const finishedTasks = computed(() => tasks.value.filter((task) => task.done === true))
const notFinishedTasks = computed(() => tasks.value.filter((task) => task.done === false))
const newTaskName = ref('')
const hideCompleted = ref(false)

const addTask = () => {
  if (newTaskName.value !== '')
    tasks.value.push({ name: newTaskName.value, done: false, subTasks: [] })
  newTaskName.value = ''
}
</script>

<template>
  <div>
    <ul>
      <div>未完了タスク</div>
      <li v-for="task in notFinishedTasks" :key="task.name">
        <div>タスク名: {{ task.name }}</div>
        <button @click="task.done = true">完了</button>
        <SubTaskList />
      </li>
      <div v-if="hideCompleted === true">
        <div>完了済タスク</div> <!-- 完了済タスクがないときはここが出ないようにしたい -->
        <li v-for="task in finishedTasks" :key="task.name">
          <div>タスク名: {{ task.name }}</div>
          <button @click="task.done = false">未完了に戻す</button>
        </li>
      </div>
    </ul>
    <div>
      <label>
        タスク名：
        <input v-model="newTaskName" type="text" />
      </label>
    </div>
    <button @click="addTask">タスクを追加</button>
    <button @click="hideCompleted = !hideCompleted">
      {{ hideCompleted ? '完了済みタスクを非表示' : '完了済みタスクを表示' }}
    </button>
  </div>
</template>

<style></style>
