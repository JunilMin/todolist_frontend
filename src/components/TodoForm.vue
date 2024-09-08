<template>
    <div class="container mt-4">
      <h2 class="text-center">Todo List</h2>
      <form @submit.prevent="submitTodo">
        <!-- Title Input Field -->
        <div class="mb-3">
          <label for="title" class="form-label">Title</label>
          <input v-model="title" type="text" class="form-control" id="title" placeholder="Enter title" />
        </div>
  
        <!-- Description Input Field -->
        <div class="mb-3">
          <label for="description" class="form-label">Description</label>
          <textarea v-model="description" class="form-control" id="description" rows="3" placeholder="Enter description"></textarea>
        </div>
  
        <!-- Submit Button -->
        <button class="btn btn-primary" type="submit">Add</button>
      </form>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        title: '',
        description: ''
      };
    },
    methods: {
      async submitTodo() {
        if (this.title === '') return;
  
        const newTodo = {
          title: this.title,
          description: this.description,
          completed: false
        };
  
        try {
          const response = await axios.post('/api/todos', newTodo);
          this.$emit('todo-added', response.data);
          this.title = '';
          this.description = '';
        } catch (error) {
          console.error('Failed to add todo:', error);
        }
      }
    }
  };
  </script>
  