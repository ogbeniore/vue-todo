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

      <TodoList :todos="list" @delete="deleteTodo" @edit="editTodo" @sort="sortList" />
      <div class="p-4" v-show="!showForm">
        <b-button @click="showForm = true" block variant="primary">Add Todo</b-button>
      </div>
       <br><br><br> 
      <TodoForm 
        :selected="currentTodo"
        @addTodo="addNewTodo"
        @updateTodo="updateTodo"
        @closeForm="resetForm" 
        v-show="showForm"/>
    </div>
  </div>
</template>

<script>
import TodoList from './TodoList'
import TodoForm from './TodoForm'

export default {
  name: "TodoWrapper",
  components: {
    TodoList,
    TodoForm
  },
  data() {
    return {
      showForm: false,
      list: [],
      currentTodo: {
        id: null,
        title: null,
        isComplete: false
      }
    }
  },
  computed: {
    highestId() {
      let idArray = []
      this.list.forEach(item => {
        idArray.push(item.id)
      })
      return idArray.length ? Math.max(...idArray) : 0
    }
  },
  mounted() {
    const localList = localStorage.getItem('todolist')
    if(localList) {
      this.list = JSON.parse(localList)
    } else {
      alert('Error while fetching data')
    }
  },
  watch: {
    list: {
      deep: true,
      handler: function(value) {
        localStorage.setItem('todolist',JSON.stringify(value))
      } 
    }
  },
  methods: {
    addNewTodo(value) {
      const todoObject = {
        id: this.highestId + 1,
        title: value,
        isComplete: false
      }
      this.list.push(todoObject)
    },
    deleteTodo(id) {
      let todoList = this.list.filter(item => item.id !== id)
      this.list = todoList
    },
    editTodo(id) {
      const selectedTodo = this.list.find(item => item.id === id)
      this.currentTodo = selectedTodo
      this.showForm = true
    },
    updateTodo(value) {
      let todo = {
        ...this.currentTodo, title: value
      }
      let currentIndex = todo.id - 1
      this.list.splice(currentIndex, 1, todo)
      this.resetForm()
    },
    resetForm() {
      this.showForm = false
      this.currentTodo = {
        id: null,
        title: null,
        isComplete: false
      }
    },
    sortList() {
      this.list.sort(function(a,z) {
        if(a.isComplete && !z.isComplete) {
          return -1
        } else {
          return 1
        }
      })
    }
  }
}
</script>

<style scoped></style>