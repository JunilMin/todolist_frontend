<template>
    <div class="container mt-4">
      <h1 class="text-center">Todo List</h1>
      <ul class="list-group" v-if="todos.length">
        <li class="list-group-item d-flex justify-content-between align-items-center"
            v-for="todo in todos" :key="todo.id">
          <!-- Title -->
          <div>
            <span :style="{ textDecoration: todo.completed ? 'line-through' : 'none' }">{{ todo.title }}</span>
            <p>{{ todo.description || 'No description provided' }}</p> <!-- description 표시 -->
          </div>
  
          <!-- Action Buttons -->
          <div>
            <button class="btn btn-sm btn-outline-primary me-2" @click="toggleTodoStatus(todo)">Completed</button>
            <button class="btn btn-sm btn-outline-danger" @click="deleteTodo(todo.id)">Delete</button>
          </div>
        </li>
      </ul>
      <p v-else class="text-center">No todos available</p>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        todos: []
      };
    },
    mounted() {
      this.fetchTodos();
    },
    methods: {
      async fetchTodos() {
        try {
          const response = await axios.get('/api/todos');
          this.todos = response.data;
        } catch (error) {
          console.error('Failed to fetch todos:', error);
        }
      },
      async toggleTodoStatus(todo) {
        try {
          const response = await axios.put(`/api/todos/${todo.id}`, null, {
            params: {
              completed: !todo.completed
            }
          });
          todo.completed = response.data.completed;
        } catch (error) {
          console.error('Failed to update todo status:', error);
        }
      },
      async deleteTodo(id) {
        try {
          await axios.delete(`/api/todos/${id}`);
          this.todos = this.todos.filter(todo => todo.id !== id);
        } catch (error) {
          console.error('Failed to delete todo:', error);
        }
      }
    }
  };
  </script>
  