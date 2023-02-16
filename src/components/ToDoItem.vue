<template>
  <li>
    <v-card style="height: 100%; width:100%" elevation="2" outlined>
      <v-card-title>{{ todo.title }}</v-card-title> 
      <v-card-text>{{ todo.description }}</v-card-text>
      <v-btn class="itemBtn" color="#f0f165" x-small fab @click="$emit('patch-todo', todo.id)">
        <v-icon> mdi-pencil </v-icon>
      </v-btn>

      <v-btn class="itemBtn" color="#e94b4b" x-small fab @click="$emit('delete-todo', todo.id)">
        <v-icon> mdi-delete </v-icon>
      </v-btn>
    </v-card>
  </li>
</template>

<script>
export default {
  props: ["todo"],
  methods: {
  },
};
</script>

<style scoped>
li {
  display: flex;
  justify-content: space-between;
  padding: 0.5rem 2rem;
  margin-bottom: 1rem;
}
.itemBtn{
  margin-bottom:5px;
  margin-left: 10px;
}
.complete {
  text-decoration: line-through;
}
input {
  margin-right: 1rem;
}
</style>
