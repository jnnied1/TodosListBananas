<template>
  <div class="ma-5">
    <h1 class="text-center">ToDos</h1>

    <!-- Add Task -->

    <v-text-field v-model="newTask" label="Add Task"></v-text-field>
    <v-btn
        class="mx-2"
        fab
        dark
        small
        color="red"
        @click="addTask">
      <v-icon>
        mdi-plus
      </v-icon>
    </v-btn>

    <!-- Search -->

    <v-text-field v-model="search" label="Search" class="mt-3"></v-text-field>

    <!-- Table -->

    <v-simple-table class="ma-5 border pa-1" height="350px">
      <template v-slot:default>
        <thead>
        <tr>
          <th class="text-left">
            Tasks
          </th>
          <th></th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="item in filteredRows"
            :key="item.name">
          <td :class="item.validate ? 'text-decoration-line-through' : ''">{{ item.name }}</td>
          <td>
            <div>
              <v-btn
                  fab
                  dark
                  x-small
                  :color="item.validate ? 'teal lighten-4' : 'green'"
                  @click="validateTask(item)">
                <v-icon>
                  mdi-thumb-up
                </v-icon>
              </v-btn>

              <!-- Edit Modal -->

              <v-dialog v-model="dialog">
                <template v-slot:activator="{ on, attrs }">
                  <v-btn
                      class="ma-3"
                      fab
                      dark
                      x-small
                      v-bind="attrs"
                      v-on="on"
                      @click="openModal(item)"
                      color="blue">
                    <v-icon>
                      mdi-pencil
                    </v-icon>
                  </v-btn>
                </template>

                <v-card>
                  <v-card-title class="text-h5 grey lighten-2 mb-2">
                    Update this Task : "{{current.name}}"
                  </v-card-title>

                  <v-card-text>
                    <v-text-field v-model="newName" label="Edit"></v-text-field>
                  </v-card-text>

                  <v-divider></v-divider>

                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn
                        color="primary"
                        text
                        @click="updateTask(current)">
                      Update
                    </v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>

              <!-- End Edit Modal -->

              <v-btn
                  fab
                  dark
                  x-small
                  color="red"
                  @click="deleteTask(item)">
                <v-icon>
                  mdi-delete
                </v-icon>
              </v-btn>
            </div>
          </td>
        </tr>
        </tbody>
      </template>
    </v-simple-table>
  </div>
</template>

<script>
export default {
  data () {
    return {
      newTask: '',
      search: '',
      dialog: false,
      current: '',
      newName : '',
      tasks: [
        {
          name: 'Finir ce projet',
          validate: false,
        },
        {
          name: 'Aller boite aux lettres',
          validate: false,
        },
        {
          name: 'Regarder vidéo admin linux',
          validate: false,
        },
        {
          name: 'Finir projet SolidJS',
          validate: false,
        }
      ],
    }
  },
  computed: {
    filteredRows() {
      return this.tasks.filter(row => {
        let name = row.name.toLowerCase();
        let searchTerm = this.search.toLowerCase();

        return name.includes(searchTerm);
      });
    }
  },
  methods: {
    addTask() {
      if(this.newTask){
        this.tasks.push({name : this.newTask, validate: false});
        this.newTask = "";
      }
    },
    deleteTask(item) {
      let pos = this.tasks.indexOf(item);
      if (pos > -1) {
        this.tasks.splice(pos, 1);
      }
    },
    validateTask(item) {
      item.validate = !item.validate;
    },
    openModal(item) {
      this.current = item;
    },
    updateTask(item) {
      this.dialog = false;
      if(this.newName)
        item.name = this.newName;
    },
  },
}
</script>

<style scoped>

  .border{
    border: solid black 1px;
  }

</style>


