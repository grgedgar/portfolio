<template>
  <div>
    <todo v-on:delete-todo="deleteTodo" v-on:save-todos="saveTodos" v-on:complete-todo="completeTodo" v-on:uncomplete-todo="uncompleteTodo" v-for="todo in todos" :todo.sync="todo" :key="todo.id"></todo>
  </div>
</template>

<script type = "text/javascript" >

import Todo from './Todo';

export default {
  props: ['todos'],
  components: {
    Todo,
  },
  methods: {
    deleteTodo(todo) {
      const localstorageTodos = JSON.parse(localStorage.getItem('todos_all'));
      const index = localstorageTodos.findIndex(o => o.uid === todo.uid);
      if (index !== -1) {
        localstorageTodos.splice(index, 1);
      }
      localStorage.setItem('todos_all', JSON.stringify(localstorageTodos));
      window.location.reload();
    },
    completeTodo(todo) {
      const todoIndex = this.todos.indexOf(todo);
      this.todos[todoIndex].done = true;
      this.$emit('save-todos');
      window.location.reload();
    },
    uncompleteTodo(todo) {
      const todoIndex = this.todos.indexOf(todo);
      this.todos[todoIndex].done = false;
      this.$emit('save-todos');
      window.location.reload();
    },
    saveTodos() {
      this.$emit('save-todos');
    },
  },
};

</script>
