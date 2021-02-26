<template>
  <div>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <div class="container-fluid">
        <a class="navbar-brand" href="#">Todo Lists</a>
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarNavDropdown"
          aria-controls="navbarNavDropdown"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
      </div>
    </nav>
    <div class="m-2">
      <TodoList :todos="list" @delete="deleteTodo" @edit="editForm" />

      <div class="p-4" v-if="!showForm">
        <b-button @click="showForm = true" block variant="primary">Add Todo</b-button>
      </div>

      <TodoForm @addTodo="addNewTodo" @closeForm="showForm = false" v-else />
      <EditForm :selected="currentTodo" v-show="showEditForm" @closeForm="showEditForm = false" @updateTodo="updateTodo" />
    </div>
  </div>
</template>

<script>
import TodoList from './TodoList'
import TodoForm from './TodoForm'
import EditForm from './EditForm'

export default {
  name: "TodoWrapper",
  components: {
    TodoList,
    TodoForm,
    EditForm
  },
  data() {
    return {
      showForm: false,
      showEditForm: false,
      list: [
        {
          id: 1,
          title: "Wake Up",
          isComplete: false
        },
        {
          id: 2,
          title: "Eat",
          isComplete: false
        },
        {
          id: 3,
          title: "Code",
          isComplete: false
        },
        {
          id: 4,
          title: "Rinse and Repeat",
          isComplete: false
        }
      ],
      currentTodo: {
        id: null,
        title: null,
        isComplete: false
      }
    }
  },
  methods: {
    addNewTodo(value) {
      const todoObject = {
        id: this.list.length + 1,
        title: value,
        isComplete: false
      }

      this.list.push(todoObject)
    },
    deleteTodo(id) {
      let todoList = this.list.filter(item => item.id !== id)
      this.list = todoList
    },
    editForm(id) {
      let selectedTodo = this.list.find(item => item.id === id)
      this.currentTodo = selectedTodo
      this.showEditForm = true
    },
    updateTodo() {
      this.showEditForm = false
      this.currentTodo =  {
        id: null,
        title: null,
        isComplete: false
      }
    }
  }
}
</script>

<style scoped></style>