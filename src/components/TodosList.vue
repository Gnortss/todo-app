<template>
  <v-container>
    <v-text-field type="text" name="listName" id="listName" v-model="newListName"></v-text-field>
    <v-btn type="submit" @click="addList">Add new list</v-btn>
    <v-list-item v-for="{name, id} in list" :key="id" @click="$emit('select:list', id)">
      <v-list-item-title>{{ name }}</v-list-item-title>
      <v-btn @click.stop="$emit('remove:list', id)">Remove</v-btn>
    </v-list-item>
  </v-container>
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

<style scoped></style>
