<template>
  <div class="home">
    <v-text-field
        v-model="newTaskTitle"
        @click:append="addTask"
        @keyup.enter="addTask"
        class="pa-3"
        outlined
        label="Add task"
        append-icon="mdi-plus"
        hide-details
        clearable
    ></v-text-field>

    <v-list
      flat
      class="pt-0"
    >
      <div
          v-for="task in tasks"
          :key="task.id"
      >
        <v-list-item
            @click="doneTask(task.id)"
            :class="{'blue lighten-5': task.done}"
        >
          <template v-slot:default>
            <v-list-item-action>
              <v-checkbox
                  :input-value="task.done"
                  color="primary"
              ></v-checkbox>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title
                  :class="{'text-decoration-line-through': task.done}"
              >
                {{task.title}}
              </v-list-item-title>
            </v-list-item-content>
            <v-list-item-action>
              <v-btn
                  icon
                  @click.stop="deleteTask(task.id)"
              >
                <v-icon color="red">mdi-delete</v-icon>
              </v-btn>
            </v-list-item-action>
          </template>

        </v-list-item>
        <v-divider></v-divider>
      </div>
    </v-list>
    <h1>Todo page</h1>
  </div>
</template>

<script>

export default {
  name: 'Todo',
  data ()  {
    return {
      newTaskTitle: '',
      tasks: [],
    }
  },

  mounted () {
    if (localStorage.getItem('tasks')){
      try {
        this.tasks = JSON.parse(localStorage.getItem('tasks'))
      } catch (e) {
        localStorage.removeItem('tasks')
      }
    }
  },

  methods: {
    addTask () {
      let newTask = {
        id: Date.now(),
        title: this.newTaskTitle,
        done: false
      }
      this.tasks.push(newTask)
      this.newTaskTitle = ""
      this.savetasks()
    },

    doneTask(id) {
      let task = this.tasks.filter(task => task.id === id)[0]
      task.done = !task.done
      this.savetasks()
    },

    deleteTask(id) {
      this.tasks = this.tasks.filter(task => task.id !== id)
      this.savetasks()
    },

    savetasks () {
      const parsed = JSON.stringify(this.tasks)
      localStorage.setItem('tasks', parsed)
    }
  }

}
</script>
