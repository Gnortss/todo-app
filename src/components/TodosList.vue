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
        class="todos lighten-2"
        :class="color"
        :ripple="false"
        v-for="{name, color, todos, id} in list" :key="id"
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
    <v-footer fixed>
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
            <v-form v-model="valid">
              <v-text-field v-model="newListName" :rules="nameRules" required autofocus></v-text-field>
              <v-select v-model="selectedColor" :items="colors">
                <template v-slot:selection="{ item }">
                  <v-chip large dark class="lighten-2" :class="selectedColor">
                    <span>{{ item }}</span>
                  </v-chip>
                </template>
              </v-select>
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="primary" text @click="addList">Add</v-btn>
          </v-card-actions>
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
      selectedColor: "red",
      dialog: false,
      valid: false,
      newListName: '',
      nameRules: [
        v => v.length >= 3 || 'Minimum 3 characters!'
      ],
      colors: [ "red", "pink", "blue", "green", "orange" ]
    }
  },
  methods: {
    addList: function() {
      if(this.newListName === '') return
      this.$emit('add:list', { name: this.newListName, color: this.selectedColor });
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
