<template>
  <v-app>
    <div id="app">
      <h1>ToDo App</h1>
      <hr />
      <br />
      <v-container>
        <v-layout style="margin-right: 10px" row wrap>
          <v-col col="7">
            <TodoList
              v-bind:todos="todoList"
              @delete-todo="deleteTodo"
              @patch-todo="getPatchTodo"
            />
          </v-col>
          <v-col col="3">
            <v-btn
              style="width: 100px"
              color="success"
              class="list-control"
              @click="getTodos()"
              >Обновить
            </v-btn>
            <br />
            <v-btn
              style="width: 100px"
              color="yellow"
              class="list-control"
              @click="clickCreateBtn"
            >
              Создать
            </v-btn>
            <br />
            <v-btn
              style="width: 100px"
              color="error"
              class="list-control"
              @click="deleteTodos()"
            >
              Удалить
            </v-btn>
          </v-col>
          <v-col style="margin-top: 10px" col="2">
            <v-text-field
              width="400px"
              label="Title"
              v-model="inputTitle"
              outlined
              dense
              class="shrink"
            >
            </v-text-field>
            <v-text-field
              width="400px"
              label="Description"
              v-model="inputDescription"
              outlined
              dense
              class="shrink"
            >
            </v-text-field>
            <v-btn
              v-if="createFlag"
              @click="createTodo()"
              vclass="patchBtn"
              style="margin-right: 10px"
              small
              color="cyan"
              >Create
            </v-btn>
            <v-btn v-if="patchFlag" @click="updateTodo()" vclass="patchBtn" small color="success"
              >Save
            </v-btn>
          </v-col>
        </v-layout>
      </v-container>
    </div>
  </v-app>
</template>

<script>
const axios = require("axios");
import TodoList from "@/components/ToDoList.vue";
import addTodo from "@/components/AddToDo.vue";
export default {
  name: "App",
  components: {
    TodoList,
    addTodo,
  },

  data: () => ({
    createFlag: true,
    patchFlag: false,
    todoList: [],
    inputTitle: "",
    inputDescription: "",
    changeId: "",
  }),
  mounted: function () {
    this.getTodos();
  },

  methods: {
    clickCreateBtn() {
      this.inputTitle = '';
      this.inputDescription = '';
      this.createFlag = true;
      this.patchFlag = false;
    },
    async getTodos() {
      const todos = await axios.get("http://192.168.1.64:3000/items");
      this.todoList = todos.data.todoList;
    },

    async deleteTodos() {
      await axios.delete("http://192.168.1.64:3000/items");
      this.getTodos();
    },
    async createTodo() {
      await axios.post("http://192.168.1.64:3000/items", {
        title: this.inputTitle,
        description: this.inputDescription,
        isCompleted: false,
      });
      this.inputTitle = "";
      this.inputDescription = "";
      this.getTodos();
    },
    async deleteTodo(id) {
      await axios.delete("http://192.168.1.64:3000/items/" + id);
      this.inputTitle = "";
      this.inputDescription = "";
      this.changeId = "";
      this.getTodos();
    },
    async getPatchTodo(id) {
      this.patchFlag = true;
      this.createFlag = false;
      const toDo = await axios.get("http://192.168.1.64:3000/items/" + id);
      this.inputTitle = toDo.data.todoById.title;
      this.inputDescription = toDo.data.todoById.description;
      this.changeId = id;
    },
    async updateTodo() {
      await axios.patch("http://192.168.1.64:3000/items/" + this.changeId, {
        title: this.inputTitle,
        description: this.inputDescription,
      });
      this.inputTitle = "";
      this.inputDescription = "";
      this.changeId = "";
      this.getTodos();
    },
  },
};
</script>

<style>
.list-control {
  margin-top: 10px;
  margin-left: 15px;
}
#btn1 {
  margin-bottom: 10px;
}
#app {
  font-family: "Avenir", Arial, Helvetica, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothig: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 10px;
}
</style>
