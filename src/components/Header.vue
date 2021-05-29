<template>
  <header class="flex">
    <h1>{{ title }}</h1>
    <Button
      v-show="isHomePage"
      @click="$emit('toggle-add-task')"
      :color="showAddTask ? 'red' : 'green'"
      :text="showAddTask ? 'Close' : 'Add Task'"
    />
  </header>
</template>

<script>
  import Button from "./Button";

  export default {
    name: "Header",
    props: {
      title: String,
      showAddTask: Boolean
    },
    components: {
      Button
    },
    computed: {
      isHomePage() {
        return this.$route.path === "/";
      }
    }
  }
</script>

<style scoped>
  header {
    align-items: center;
    justify-content: space-between;
    margin-bottom: 20px;
  }
</style>