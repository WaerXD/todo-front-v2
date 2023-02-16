<template>
  <div>
    <ul>
      <TodoItem
        v-for="todo of todos"
        v-bind:todo="todo"
        @delete-todo="deleteTodo"
        @patch-todo="patchTodo"
      />
    </ul>
  </div>
</template>

<script>
import TodoItem from "@/components/ToDoItem.vue";
export default {
  props: ["todos"],
  components: {
    TodoItem,
  },
  methods: {
    deleteTodo(id) {
      this.$emit("delete-todo", id);
    },
    patchTodo(id) {
      this.$emit("patch-todo", id);
    },
  },
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
