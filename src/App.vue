<template>
  <div class="container">
    <Header title="Task Tracker" @toggle-add-task="toggleAddTask" :showAddTask="showAddTask" />
    <div v-if="showAddTask">
      <AddTask @addNewTask="addTask" />
    </div>
    <Tasks :tasks="tasks" @delete-task="deleteTask" @toggle-remainder="toggleRemainder" />
  </div>
</template>

<script>
import Header from './components/Header.vue';
import Tasks from './components/Tasks.vue';
import AddTask from './components/AddTask.vue';

const backendUrl = 'http://localhost:5000/';

export default {
  name: 'App',
  components: {
    Header,
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [],
      showAddTask: false,
    };
  },
  methods: {
    toggleAddTask() {
      this.showAddTask = !this.showAddTask;
    },
    async addTask(task) {
      const res = await fetch(backendUrl + 'tasks', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(task),
      });
      const data = await res.json();
      if (res.ok) this.tasks = this.tasks.concat(data);
      else alert('Error Adding Task');
      this.fetchTasks();
    },
    async deleteTask(id) {
      if (confirm('Are you sure you want to delete')) {
        const res = await fetch(`${backendUrl}tasks/${id}`, {
          method: 'DELETE',
        });
        // const data = await res.json();
        if (res.ok) this.tasks = this.tasks.filter((t) => t.id !== id);
        else alert('Error Deleting Task');
        this.fetchTasks();
      }
    },
    async toggleRemainder(id) {
      const targetTask = this.tasks.filter((t) => t.id === id)[0];
      const updated = {...targetTask, reminder: !targetTask.reminder};

      const res = await fetch(`${backendUrl}tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(updated),
      });
      if (res.ok) {
        this.tasks = this.tasks.map((e) => (e.id === id ? {...e, reminder: !e.reminder} : e));
        this.fetchTasks();
      } else alert('Error Toggling Remainder');
    },
    async fetchTasks() {
      const res = await fetch(backendUrl + 'tasks');
      const data = await res.json();
      this.tasks = data;
    },
  },
  created() {
    this.fetchTasks();
  },
};
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
