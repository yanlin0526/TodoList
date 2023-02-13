<template>
  <div class="todo-list">
    <TodoInput />
    <div class="list-container">
      <TodoItem v-for="todo in todos" :key="todo.id" :task="todo" />
    </div>
    <div class="footer">
      <span>{{ taskLeft }} left</span>
      <div class="filter-buttons">
        <button
          v-for="filterType in filterTypes"
          :key="filterType"
          :class="{ active: filter === filterType }"
          @click="setFilter(filterType)"
        >
          {{ filterType }}
        </button>
      </div>

      <button @click="clearCompleted">Clear Completed</button>
    </div>
  </div>
</template>

<script>
import TodoInput from "./TodoInput.vue";
import TodoItem from "./TodoItem.vue";

import { computed } from "vue";
import { useStore } from "vuex";

import { useTodoList } from "../composables/useTodoList";
import { useQuasar } from "quasar";

export default {
  components: { TodoInput, TodoItem },
  setup() {
    const store = useStore();
    const { setFilter, todos, filter } = useTodoList();
    const $q = useQuasar();

    const taskLeft = computed(function () {
      return store.getters["todos/active"].length;
    });

    function clearCompleted() {
      $q.dialog({
        title: "CONFIRM",
        message: "Do you want to clear completed tasks??",
        cancel: true,
        persistent: true,
      }).onOk(() => {
        store.commit("todos/clearCompleted");
      });
    }

    return {
      todos,
      filter,
      taskLeft,
      filterTypes: ["All", "Active", "Completed"],
      setFilter,
      clearCompleted,
    };
  },
};
</script>

<style lang="scss">
.todo-list {
  position: absolute;
  font-size: 18px;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 60vw;
  height: 50vh;
  display: flex;
  flex-direction: column;
  .list-container {
    flex-grow: 1;
    overflow: auto;
    background: var(--todo-bg-color);
    border-radius: 1rem;
    padding: 0.7rem;
    margin-top: 0.5rem;
    margin-bottom: 0.5rem;
  }
  .footer {
    background: var(--todo-bg-color);
    display: flex;
    border-radius: 1rem;
    padding: 1rem;
    align-items: center;
    color: var(--text-secondary);
    button {
      background: none;
      border: none;
      color: inherit;
      margin-right: 2rem;
      cursor: pointer;
    }
    .filter-buttons {
      margin-left: 5rem;
      flex-grow: 1;
      button.active {
        color: #ffffff;
      }
    }
  }
}
</style>
