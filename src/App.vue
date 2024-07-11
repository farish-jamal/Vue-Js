<script setup>
import { onMounted, ref } from 'vue';
const name = ref("Farish");
const status = ref("inactive");
const tasks = ref(["task one", "task two", "task three", "task four"]);
const links = ref("https://google.com");
const todo = ref('');

const toggle = () => {
  if (status.value === "active") {
    status.value = "inactive";
  } else if (status.value === "inactive") {
    status.value = "active";
  } else {
    status.value = "active";
  }
};

const addTask = () => {
  if(todo.value !== ''){
    tasks.value.push(todo.value);
    todo.value = '';
  }
}

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
}

onMounted(async () => {
  try {
    const response =  await fetch("https://jsonplaceholder.typicode.com/todos");
    const data = await response.json();
    tasks.value =  data.map((task) => task.title);
  } catch (error) {
    console.error("Error Fetching the data")
  }
})
</script>

<template>
  <h1>{{ name }}</h1>
  <p v-if="status === 'active'">active</p>
  <p v-else-if="status === 'pending'">pending</p>
  <p v-else>inactive</p>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
     <span>{{ task }}</span>
     <button @click="deleteTask(index)">X</button>
    </li>
  </ul>
  <a :href="links">Click for google</a>
  <button @click="toggle">Toggle Status</button>
  <form @submit.prevent="addTask">
    <input type="text" name="todo" id="todo" v-model="todo">
    <button type="submit">Add Task</button>
  </form>
</template>

<style scoped>
h1 {
  color: red;
}

p {
  color: green;
}

ul>li {
  text-transform: capitalize;
}
</style>
