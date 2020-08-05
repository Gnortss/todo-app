<template>
  <v-app>
    <v-app-bar app dense>
      <template v-if="selected !== null">
        <v-btn icon v-if="selected !== null" @click.stop="selected=null"><v-icon>arrow_back_ios</v-icon></v-btn>
        <v-toolbar-title>{{ selected.name }}</v-toolbar-title>
      </template>
      <template v-else>
        <v-toolbar-title>My todos</v-toolbar-title>
      </template>
    </v-app-bar>
    <v-main>
      <TodosList v-if="selected === null" :list="todosList" 
        @remove:list="removeList($event)" 
        @select:list="selected=todosList.find(todos => todos.id === $event)"
        @add:list="addList($event)"/>
      <Todos v-else :name="selected.name" :todos="selected.todos"
        @update="updateTodosListLocalStorage(todosList)"
        @remove:todo="removeTodo($event)"
        @add:todo="addTodo($event)"/>
    </v-main>
  </v-app>
</template>

<script>
import { v4 as uuidv4 } from 'uuid';
import TodosList from "./components/TodosList";
import Todos from "./components/Todos";

export default {
  name: "App",
  components: {
    TodosList,
    Todos
  },
  mounted() {
    if(localStorage.todosList) {
      this.todosList = JSON.parse(localStorage.todosList || "[]");
    }
  },
  watch: {
    todosList(newTodosList) {
      this.updateTodosListLocalStorage(newTodosList);
    }
  },
  methods: {
    addList: function({ name, color }) {
      this.todosList = [...this.todosList, { id: uuidv4(), name, color, todos: [] }];
    },
    removeList: function(id) {
      this.todosList = this.todosList.filter((v) => v.id != id);
    },
    addTodo: function(title) {
      this.selected.todos = [...this.selected.todos, { id: uuidv4(), title, completed: false }];
      this.updateTodosListLocalStorage(this.todosList);
    },
    removeTodo: function(id) {
      this.selected.todos = this.selected.todos.filter((todo) => todo.id != id);
      this.updateTodosListLocalStorage(this.todosList);
    },
    updateTodosListLocalStorage(newTodosList) {
      localStorage.setItem("todosList", JSON.stringify(newTodosList));
    }
  },
  data() {
    return {
      listName: '',
      selected: null,
      todosList: []
    }
  }
};
</script>

<style>
</style>
