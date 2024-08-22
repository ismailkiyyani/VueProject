<script setup>
import { onMounted, ref } from 'vue';

const name = 'CR7';
const status = ref('active');
const tasks = ref(['Task One', 'Task Two', 'Task Three']);
const newTask = ref('');

const toggleButton = () => {
  if (status.value === 'active') {
    status.value = 'Pending';
  } else if (status.value === 'Pending') {
    status.value = 'inactive';
  } else {
    status.value = 'active';
  }
};
const addTask = () => {
  if (newTask.value.trim() != '') {
    tasks.value.push(newTask.value);
    newTask.value = '';
  }
};
const deleteTask = (index) => {
  tasks.value.splice(index, 1);
};
onMounted(async () => {
  const result = await fetch('https://jsonplaceholder.typicode.com/todos');
  const data = await result.json();
  tasks.value = data.map((task) => task.title);
});
const editTask = (task, index) => {
  // let index = tasks.findIndex((val) => {val === task})
  console.log(task, index);
  console.log(typeof task);
};
</script>

<template>
  <h1>{{ name }}</h1>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span>{{ task }}</span>
      <button @click="deleteTask(index)">x</button>
      <button @click="editTask(task, index)">&</button>
    </li>
  </ul>
  <br />
  <form @submit.prevent="addTask">
    <label for="newTask">Add Task </label>
    <input type="text" id="newTask" name="newTask" v-model="newTask" />
    <button type="submit">Submit</button>
  </form>
  <p v-if="status === 'active'">User is {{ status }}</p>
  <p v-else-if="status === 'Pending'">User is Pending</p>
  <p v-else>User is Inactive</p>

  <button @click="toggleButton">Click Me</button>
  <br />
</template>

<style scoped></style>
