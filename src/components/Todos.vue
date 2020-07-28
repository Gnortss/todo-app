<template>
  <div>
    <button type="submit" @click="$emit('select:null')">Back</button>
    <h1>{{ name }}</h1>
    <div v-for="{ id, title } in todos" :key="id">
      <button type="submit" @click="$emit('remove:todo', id)">Remove</button>
      {{ title }}
    </div>
    <div>
      <input type="text" name="newTodo" id="newTodo" v-model="newTodoTitle">
      <button type="submit" @click="addTodo">Add Todo</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Todos",
  props: ['name', 'todos'],
  data() {
    return {
      newTodoTitle: ''
    }
  },
  methods: {
    addTodo: function() {
      if(this.newTodoTitle === '') return
      this.$emit('add:todo', this.newTodoTitle);
      this.newTodoTitle = '';
    }
  }
}
</script>

<style>

</style>