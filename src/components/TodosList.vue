<template>
  <div>
    <slot></slot>
    <h1>List of my todos:</h1>
    <input type="text" name="listName" id="listName" v-model="newListName">
    <button type="submit" @click="addList">Add new list</button>
    <div class="todosList" v-for="{id, name} in list" :key="id" @click.self="$emit('select:list', id)">
      <button @click="$emit('remove:list', id)">Remove</button>
      {{ name }}
    </div>
  </div>
</template>

<script>
export default {
  name: "TodosList",
  props: ["list"],
  data() {
    return {
      newListName: ''
    }
  },
  methods: {
    addList: function() {
      if(this.newListName === '') return
      this.$emit('add:list', this.newListName);
      this.newListName = '';
    }
  }
}
</script>

<style scoped>
.todosList {
  border: 1px solid black;
}
</style>
