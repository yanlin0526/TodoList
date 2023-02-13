<template>
  <div class="content">
    <Title />
    <TodoList />
  </div>
</template>

<script setup>
import Title from "./components/Title.vue";
import TodoList from "./components/TodoList.vue";
</script>

<style lang="scss">

:root {
  --background-color: #630041;
  --text-primary: #ffffff;
  --text-secondary: #000000;
  --todo-bg-color: #ed1299;
}
body {
  margin: 0px;
}
.content {
  background: var(--background-color);
  height: 100vh;
  overflow: hidden;
  position: relative;
}
</style>
