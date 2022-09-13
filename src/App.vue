<template>
  <div class="container">
    <HeaderComponent @toggle-add-task="toggleAddTask" :showAddTask="showAddTask" title="Task Tracker"/>
    <div v-if="showAddTask">
      <AddTaskComponent @add-task="addTask"/>
    </div>
    <TaskComponent :tasks="tasks" @delete-task="deleteTask" @toggle-reminder="toggleReminder"/>
  </div>
</template>

<script>
import HeaderComponent from './components/Header.vue';
import TaskComponent from './components/Tasks.vue';
import AddTaskComponent from './components/AddTask.vue';

export default {
  name: 'App',
  components: {
    HeaderComponent,
    TaskComponent,
    AddTaskComponent
  },
  data() {
    return {
      tasks: [],
      showAddTask: false
    }
  },
  methods: {
    async deleteTask (id) {
      if(confirm('Are you sure?')) {
        const response = await fetch(`http://localhost:5000/tasks/${id}`, {
          method: 'DELETE'
        });

        response.status === 200 ? (this.tasks = this.tasks.filter((task) => task.id !== id)) : alert('Error deleting task');

       
      }
    },

    async toggleReminder (id) {
      // const taskIndex = parseInt(id-1);
      // this.tasks[taskIndex].reminder = !this.tasks[taskIndex].reminder;
      // or
      // this.tasks = this.tasks.map((task) => task.id === id ? { ...task, reminder: !task.reminder} : task);

      const taskToToggle = await this.fetchSingleTask(id);
      const updatedTask = {...taskToToggle, reminder: !taskToToggle.reminder};

      const response = await fetch(`http://localhost:5000/tasks/${id}`, {
          method: 'PUT',
          headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(updatedTask)
      });

      const data = await response.json();

      this.tasks = this.tasks.map((task) => task.id === id ? { ...task, reminder: data.reminder} : task);
    },

    async addTask(newTask) {
      const response = await fetch('http://localhost:5000/tasks', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(newTask)
      });

      const data = await response.json();

      this.tasks = [...this.tasks, data];
    },

    toggleAddTask() {
      this.showAddTask = !this.showAddTask;
    },

    async fetchTasks() {
      const response = await fetch('http://localhost:5000/tasks');

      const data = await response.json();

      return data;
    },

    async fetchSingleTask(id) {
      const response = await fetch(`http://localhost:5000/tasks/${id}`);

      const data = await response.json();

      return data;
    }
  },
  async created() {
    this.tasks = await this.fetchTasks();
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Poppins', sans-serif;
}

.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}

.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}

.btn:focus {
  outline: none;
}

.btn:active {
  transform: scale(0.98);
}

.btn-block {
  display: block;
  width: 100%;
}
</style>
