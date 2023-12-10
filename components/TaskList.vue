<script setup lang="ts">
import { computed, ref } from 'vue';
import SubTaskList from './SubTaskList.vue';

interface SubTask {
  name: string;
  done: boolean;
}
interface Task {
  name: string;
  done: boolean;
  subTasks: SubTask[];
}

const tasks = ref<Task[]>([
  { name: 'A', done: false, subTasks: [] },
  { name: 'B', done: false, subTasks: [] },
]);
const finishedTasks = computed(() => tasks.value.filter((task) => task.done));
const notFinishedTasks = computed(() =>
  tasks.value.filter((task) => !task.done),
);
const newTaskName = ref('');
const showState = ref('all');

const addTask = () => {
  if (newTaskName.value !== '')
    tasks.value.push({ name: newTaskName.value, done: false, subTasks: [] });
  newTaskName.value = '';
};
</script>

<template>
  <div>
    <ul>
      <div v-if="showState === 'all'">
        <div>すべてのタスク</div><!-- 一応切り替えられるけど微妙いかんじになってるかも -->
        <li v-for="task in tasks" :key="task.name">
          <div>タスク名: {{ task.name }}</div>
          <button @click="task.done = true">完了</button>
          <SubTaskList />
        </li>
      </div>
      <div v-if="showState === 'not'">
        <div>未完了タスク</div>
        <li v-for="task in notFinishedTasks" :key="task.name">
          <div>タスク名: {{ task.name }}</div>
          <button @click="task.done = true">完了</button>
          <SubTaskList />
        </li>
      </div>
      <div v-if="showState === 'done'">
        <div>完了済タスク</div>
        <!-- 完了済タスクがないときはここが出ないようにしたい -->
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
    <div>
      <button @click="showState = 'all'">すべてのタスク</button>
      <button @click="showState = 'not'">未完了タスク</button>
      <button @click="showState = 'done'">完了済タスク</button>
    </div>
  </div>
</template>

<style></style>
