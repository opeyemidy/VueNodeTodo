<template>
  <div class="home">
    <b-container>
      <b-row>
        <b-col lg="6" class="mx-auto my-5">
          <h1 class="text-center">Todo List App</h1>
          <b-card class="mt-3 remove-mb">
            <div>
              <b-form-input
                v-model="text"
                placeholder="Enter your name"
              ></b-form-input>
            </div>
            <div class="mt-2">
              <b-button
                v-if="this.isEdit === false"
                block
                variant="success"
                @click="addNewTask"
                >Submit</b-button
              >
              <b-button v-else block @click="updateTask" variant="primary"
                >Update</b-button
              >
            </div>
            <b-container fluid class="px-0">
              <div v-for="todo in todos" :key="todo.id">
                <b-row class="mt-3 align-items-center">
                  <b-col cols="8"
                    ><h6 class="text-capitalize">
                      {{ todo.task_name }}
                    </h6></b-col
                  >
                  <b-col cols="4" class="text-right">
                    <b-button
                      variant="info"
                      size="sm"
                      class="mr-1"
                      @click="editTask(todo.task_name, todo.id)"
                      >Edit</b-button
                    >
                    <b-button
                      variant="danger"
                      size="sm"
                      @click="deleteTask(todo.id)"
                      >Delete</b-button
                    >
                  </b-col>
                </b-row>
                <hr />
              </div>
            </b-container>
          </b-card>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Home',
  components: {},
  data() {
    return {
      todos: [],
      id: '',
      text: null,
      isEdit: false
    }
  },
  mounted() {
    this.getTask()
  },
  methods: {
    getTask() {
      axios.get('http://localhost:3000/api/tasks').then(
        result => {
          console.log(result.data)
          this.todos = result.data
        },
        error => {
          console.error(error)
        }
      )
    },
    addNewTask() {
      axios
        .post('http://localhost:3000/api/task', { task_name: this.text })
        .then(res => {
          this.text = ''
          this.getTask()
        })
        .catch(err => {
          console.log(err)
        })
    },
    editTask(title, id) {
      this.id = id
      this.text = title
      this.isEdit = true
    },
    updateTask() {
      axios
        .put(`http://localhost:3000/api/task/${this.id}`, {
          task_name: this.text
        })
        .then(res => {
          this.taskname = ''
          this.isEdit = false
          this.getTask()
          console.log(res)
        })
        .catch(err => {
          console.log(err)
        })
    },
    deleteTask(id) {
      axios
        .delete(`http://localhost:3000/api/task/${id}`)
        .then(res => {
          this.text = ''
          this.getTask()
          console.log(res)
        })
        .catch(err => {
          console.log(err)
        })
    }
  }
}
</script>
<style scoped>
.remove-mb div:last-child hr {
  margin-bottom: 0;
}
</style>
