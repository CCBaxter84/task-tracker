<template>
  <form @submit="onSubmit" class="add-form">
    <section class="form-control">
      <label>Task</label>
      <input type="text" name="text" v-model="text" placeholder="Add Task">
    </section>
    <section class="form-control">
      <label>Day & Time</label>
      <input type="text" name="name" v-model="day" placeholder="Add Day & Time">
    </section>
    <section class="form-control form-control-check">
      <label>Set Reminder</label>
      <input type="checkbox" name="reminder" v-model="reminder" placeholder="Set Reminder">
    </section>
    <input type="submit" class="btn btn-block" value="Save Task">
  </form>
</template>

<script>
  export default {
    name: "AddTask",
    data() {
      return {
        text: "",
        day: "",
        reminder: false
      }
    },
    methods: {
      onSubmit(event) {
        event.preventDefault(); // Prevent page refresh
        if (this.formHasError()) {
          alert("Please add a task");
          return;
        } else {
          this.submitNewTask();
          this.resetFormData();
        }
      },
      formHasError() {
        return !this.text;
      },
      submitNewTask() {
        const newTask = {
          text: this.text,
          day: this.day,
          reminder: this.reminder
        }
        this.$emit("add-task", newTask);
      },
      resetFormData() {
        this.text = "";
        this.day = "",
        this.reminder = false;
      }
    }
  }
</script>

<style scoped>
  .add-form {
    margin-bottom: 40px;
  }
  .form-control {
    margin: 20px 0;
  }
  .form-control label {
    display: block;
  }
  .form-control input {
    width: 100%;
    height: 40px;
    margin: 5px;
    padding: 3px 7px;
    font-size: 17px;
  }
  .form-control-check {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  .form-control-check label {
    flex: 1;
  }
  .form-control-check input {
    flex: 2;
    height: 20px;
  }
</style>