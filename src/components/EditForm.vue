<template>
  <div class="card p-4 text-left">
    <form action="" @submit.prevent="updateTodo">
      <h2>Edit Todo</h2>
      <div class="mb-3">
        <label for="todo" class="form-label">Todo title</label>
        <input
          v-model="selected.title"
          type="text"
          class="form-control"
          id="todo"
          placeholder="Name of Todo"
        />
      </div>
      <div>
        <button 
          class="btn btn-outline-primary"
          variant="outline-primary" 
          type="submit"
          :disabled="!selected.title"
          >
          Edit Todo
        </button>
        <button class="btn" variant="danger" type="button" @click="$emit('closeForm')">Cancel</button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: 'EditForm',
  props: {
    selected: {
      type: Object,
      required: true
    }
  },
  methods: {
    updateTodo() {
      this.$emit('updateTodo')
    }
  }
}
</script>