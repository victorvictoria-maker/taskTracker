<template>
    <AddTaskComponent v-if="showAddTask" @add-task="addTask"/>
    <TaskComponent :tasks="tasks" @delete-task="deleteTask" @toggle-reminder="toggleReminder"/>
</template>


<script>
import AddTaskComponent from '../components/AddTask.vue';
import TaskComponent from '../components/Tasks.vue';

export default {
    name: 'HomeView',
    components: {
        TaskComponent,
        AddTaskComponent,
    },
    data() {
        return {
            tasks: []
        }
    },
    props: {
        showAddTask: Boolean
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

</style>