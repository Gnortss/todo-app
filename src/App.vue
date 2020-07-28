<template>
  <div id="app">
    <TodosList v-if="selected === null" :list="todosList" 
      @remove:list="removeList($event)" 
      @select:list="selected=todosList.find(todos => todos.id === $event)"
      @add:list="addList($event)">
      <input type="text" name="listName" id="todosName" v-model="listName">
      <button type="submit" @click="addList">Create new List</button>
    </TodosList>
    <Todos v-else :name="selected.name" :todos="selected.todos"
      @remove:todo="removeTodo($event)" 
      @select:null="selected=null"
      @add:todo="addTodo($event)"/>
  </div>
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
    addList: function(name) {
      this.todosList = [...this.todosList, { id: uuidv4(), name, todos: [] }];
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

<style></style>
