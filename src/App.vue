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
      addTask(task) {
        this.tasks = [...this.tasks, task];
      },
      deleteTask(id) {
        this.tasks = this.tasks.filter(task => task.id !== id);
      },
      toggleAddTask() {
        this.showAddTask = !this.showAddTask;
      },
      toggleReminder(id) {
        this.tasks = this.tasks.map(task => {
          if (task.id === id) {
            return {
              ...task,
              reminder: !task.reminder
            }
          } else {
            return task
          }
        });
      }
    },
    created() {
      this.tasks = [
        {
          id: 1,
          text: "Take dogs squirrel hunting",
          day: "June 1st at 5:30pm",
          reminder: false
        },
        {
          id: 2,
          text: "Cook dinner",
          day: "June 2nd at 7:00pm",
          reminder: true
        }
      ]
    }
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
