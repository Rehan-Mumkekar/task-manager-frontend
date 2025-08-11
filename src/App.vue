<template>
  <div class="container">
    <h1>My Task Manager</h1>
    <form @submit.prevent="addTask">
      <input v-model="newTask.title" placeholder="Title" required />
      <input v-model="newTask.description" placeholder="Description" />
      <select v-model="newTask.status">
        <option>Pending</option>
        <option>Completed</option>
      </select>
      <button>Add Task</button>
    </form>

    <ul>
      <li v-for="task in tasks" :key="task._id">
        <strong>{{ task.title }}</strong> - {{ task.status }}
        <button @click="editTask(task)">Edit</button>
        <button @click="deleteTask(task._id)">Delete</button>
      </li>
    </ul>

    <div v-if="editing">
      <h3>Edit Task</h3>
      <input v-model="editData.title" />
      <input v-model="editData.description" />
      <select v-model="editData.status">
        <option>Pending</option>
        <option>Completed</option>
      </select>
      <button @click="updateTask">Update</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      tasks: [],
      newTask: { title: '', description: '', status: 'Pending' },
      editing: false,
      editData: {},
      apiUrl: 'https://task-manager-backend-ds88.onrender.com/tasks'
    };
  },
  methods: {
    async getTasks() {
      const res = await axios.get(this.apiUrl);
      this.tasks = res.data;
    },
    async addTask() {
      await axios.post(this.apiUrl, this.newTask);
      this.newTask = { title: '', description: '', status: 'Pending' };
      this.getTasks();
    },
    editTask(task) {
      this.editing = true;
      this.editData = { ...task };
    },
    async updateTask() {
      await axios.put(`${this.apiUrl}/${this.editData._id}`, this.editData);
      this.editing = false;
      this.getTasks();
    },
    async deleteTask(id) {
      await axios.delete(`${this.apiUrl}/${id}`);
      this.getTasks();
    }
  },
  mounted() {
    this.getTasks();
  }
};
</script>

<style>
@import "@/assets/style.css";
</style>

