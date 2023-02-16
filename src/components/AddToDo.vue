<template>
  <form @submit.prevent="saveTodo">
    <v-text-field
      width="400px"
      label="Title"
      outlined
      dense
      class="shrink patchIn"
      v-model="title"
    >
    </v-text-field>
    <v-text-field
      width="400px"
      label="Description"
      outlined
      dense
      class="shrink patchIn"
      v-model="description"
    >
    </v-text-field>
    <v-btn class="patchBtn" small color="cyan" type="submit">Save</v-btn>
  </form>
</template>

<script>
export default {
  data: () => ({
    title: "",
    description: "",
  }),
  methods: {
    saveTodo() {
      if (this.title.trim()) {
        const newTodo = {
          title: this.title,
          description: this.description,
          isCompleted: false,
        };
        this.$emit("add-todo", newTodo);
      }
    },
  },
};
</script>

<style scoped>

form{
  margin-top:10px
}
.v-text-field{
  width: 300px;
  align-self: center;
}
.patchIn {
  margin-left: 40px;
}

.patchBtn {
  margin-left: 5px;
  margin-bottom: 5px;
  margin-left: 10px;
}
</style>
