<template>
  <div id="app">
    <TodoForm @todo-added="addTodo"/>
    <TodoList ref="todoList"/>
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue';
import TodoForm from './components/TodoForm.vue';

export default {
  components: {
    TodoList,
    TodoForm
  },
  methods: {
    addTodo(newTodo) {
      this.$refs.todoList.todos.push(newTodo);
    }
  }
};
</script>
