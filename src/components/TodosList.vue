<template>
  <v-container style="width: 100vw">
    <!-- <v-text-field type="text" name="listName" id="listName" v-model="newListName"></v-text-field>
    <v-btn type="submit" @click="addList">Add new list</v-btn>
    <v-list-item v-for="{name, id} in list" :key="id" @click="$emit('select:list', id)">
      <v-list-item-title>{{ name }}</v-list-item-title>
      <v-btn @click.stop="$emit('remove:list', id)">Remove</v-btn>
    </v-list-item> -->
    <v-container class="todos-list d-flex flex-row flex-wrap justify-space-between align-content-space-between">
      <v-card 
        class="todos"
        :ripple="false"
        v-for="{name, todos, id} in list" :key="id"
        @click="$emit('select:list', id)"
      >
        <v-card-title class="text-capitalize">{{ name }}</v-card-title>
        <v-card-actions
          class="justify-space-between"
        >
          Tasks: {{ completedTasks(todos) }}/{{ todos.length }}
          <v-btn icon 
            class="red lighten-1" 
            :ripple="false"
            @click.stop="$emit('remove:list', id)"
          >
            <v-icon>remove</v-icon>
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-container>
    <v-footer absolute>
      <v-dialog v-model="dialog" @click:outside="newListName=''">
        <template v-slot:activator="{ on, attrs }">
          <v-btn icon
            class="mx-auto"
            color="primary"
            v-bind="attrs"
            v-on="on"
          >
            <v-icon>add</v-icon>
          </v-btn>
        </template>
        <v-card>
          <v-card-title><span class="headline">Gimme a name</span></v-card-title>
          <v-card-text>
            <v-container>
              <v-text-field required v-model="newListName" autofocus @keyup.enter="addList"></v-text-field>
            </v-container>
          </v-card-text>
        </v-card>
      </v-dialog>
    </v-footer>
  </v-container>
</template>

<script>
export default {
  name: "TodosList",
  props: ["list"],
  data() {
    return {
      newListName: '',
      dialog: false
    }
  },
  methods: {
    addList: function() {
      if(this.newListName === '') return
      this.$emit('add:list', this.newListName);
      this.dialog=false;
      this.newListName = '';
    },
    completedTasks: function(todos) {
      return todos.filter((task) => task.completed).length;
    }
  }
}
</script>

<style scoped>

.todos-list {
  width: 100%;
}

.todos {
  width: 40vw;
  min-height: 15vw;
  margin-bottom: 16px;
}

</style>
