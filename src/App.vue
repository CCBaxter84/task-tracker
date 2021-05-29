<template>
  <div class="container">
    <Header
      @toggle-add-task="toggleAddTask"
      title="Task Tracker"
      :showAddTask="showAddTask"
    />
    <AddTask v-show="showAddTask" @add-task="addTask"/>
    <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks= "tasks" />
  </div>
</template>

<script>
  import Header from "./components/Header";
  import Tasks from "./components/Tasks";
  import AddTask from "./components/AddTask";

  export default {
    name: "App",
    components: {
      Header,
      AddTask,
      Tasks
    },
    data() {
      return {
        tasks: [],
        showAddTask: false
      }
    },
    methods: {
      async addTask(task) {
        const postRequest = {
            method: "POST",
            headers: {
              "Content-type": "application/json",
            },
            body: JSON.stringify(task)
          }
        try {
          const response = await fetch("api/tasks", postRequest);
          const data = await response.json();
          this.addToAppData(data);
        } catch {
          alert("Error adding task");
        }
      },
      addToAppData(task) {
        this.tasks = [...this.tasks, task];
      },
      async deleteTask(id) {
        const deleteRequest = { method: "DELETE" }
        try {
          const response = await fetch(`api/tasks/${id}`, deleteRequest);
          const wasSuccessful = response.status === 200;
          if (wasSuccessful) {
            this.deleteFromAppData(id);
          } else {
            throw "You didn't say the magic word";
          }
        } catch {
          alert("Error deleting task");
        }        
      },
      deleteFromAppData(id) {
        this.tasks = this.tasks.filter(task => task.id !== id);
      },
      toggleAddTask() {
        this.showAddTask = !this.showAddTask;
      },
      async toggleReminder(id) {
        try {
          const task = await this.fetchTask(id);
          const updatedReminder = { reminder: !task.reminder }
          const patchRequest = {
            method: "PATCH",
            headers: { "Content-type": "application/json" },
            body: JSON.stringify(updatedReminder)
          }
          const response = await fetch(`api/tasks/${id}`, patchRequest);
          const data = await this.getData(response);
          this.toggleReminderInAppData(id, data);
        } catch {
          alert("Something went wrong with setting reminder");
        }
      },
      toggleReminderInAppData(id, data) {
        this.tasks = this.tasks.map(task => {
          if (task.id === id) {
            return {
              ...task,
              reminder: data.reminder
            }
          } else {
            return task
          }
        });
      },
      async fetchTasks() {
        try {
          const response = await fetch("api/tasks");
          return await this.getData(response);
        } catch {
          alert("Something went wrong fetching tasks");
        }        
      },
      async fetchTask(id) {
        const response = await fetch(`api/tasks/${id}`);
        return await this.getData(response);
      },
      async getData(response) {
        const data = await response.json();
        return data;
      }
    },
    async created() {
      this.tasks = await this.fetchTasks();
    }
  };
</script>
