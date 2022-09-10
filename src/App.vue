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
    deleteTask (id) {
      if(confirm('Are you sure?')) {
        this.tasks = this.tasks.filter((task) => task.id !== id);
      }
    },

    toggleReminder (id) {
      // const taskIndex = parseInt(id-1);
      // this.tasks[taskIndex].reminder = !this.tasks[taskIndex].reminder;
      // or
      this.tasks = this.tasks.map((task) => task.id === id ? { ...task, reminder: !task.reminder} : task);
    },

    addTask(newTask) {
      this.tasks = [...this.tasks, newTask];
    },

    toggleAddTask() {
      this.showAddTask = !this.showAddTask;
    }
  },
  created() {
    this.tasks = [
      {
        id: 1,
        text: 'Doctor Appointment',
        day: 'March 1st at 2:30pm',
        reminder: true
      },
      {
        id: 2,
        text: 'Meeting at School',
        day: 'March 3rd at 9:30pm',
        reminder: true
      },
      {
        id: 3,
        text: 'Food Shopping',
        day: 'March 1st at 11:00am',
        reminder: false
      }
    ]
  },
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
