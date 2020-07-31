<template>
  <v-container>
    <v-list-item v-for="todo in todos" :key="todo.id">
      <v-list-item-action>
        <v-checkbox v-model="todo.completed" @click.passive="$emit('update')">
          <template v-slot:label>
            <div class="ml-4" v-text="todo.title">
            </div>
          </template>
        </v-checkbox>
      </v-list-item-action>
      <v-btn type="submit" @click="$emit('remove:todo', todo.id)">Remove</v-btn>
    </v-list-item>
    <div>
      <v-text-field type="text" name="newTodo" id="newTodo" v-model="newTodoTitle"></v-text-field>
      <v-btn type="submit" @click="addTodo">Add Todo</v-btn>
    </div>
  </v-container>
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