<template>
  <TodoContent></TodoContent>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import TodoContent from './components/todo-content.vue';

export default defineComponent({
  name: 'App',
  components: {
    TodoContent,
  },
});
</script>

<style>
html, body, #app {
  margin: 0;
  min-height: 100vh;
  background-color: #F1EFF5;
}
</style>
