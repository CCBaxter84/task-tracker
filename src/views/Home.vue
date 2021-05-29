<template>
    <AddTask v-show="showAddTask" @add-task="addTask"/>
    <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks= "tasks" />
</template>

<script>
    import Tasks from "../components/Tasks"
    import AddTask from "../components/AddTask";
    export default {
        name: "Home",
        props: {
            showAddTask: Boolean
        },
        components: {
            Tasks,
            AddTask
        },
        data() {
            return {
                tasks: []
            }
        },
        methods: {
            buildRequest(type, data) {
                return {
                    method: type,
                    headers: {
                    "Content-type": "application/json",
                    },
                    body: JSON.stringify(data)
                }
            },
            async addTask(task) {
                const postRequest = this.buildRequest("POST", task);
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
            async toggleReminder(id) {
                try {
                    const task = await this.fetchTask(id);
                    const updatedReminder = { reminder: !task.reminder }
                    const patchRequest = this.buildRequest("PATCH", updatedReminder);
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
    }
</script>