<template>
  <div>
    <p>Completed Tasks: {{todos.filter(todo => {return todo.done === true}).length}}</p>
    <p>Pending Tasks: {{todos.filter(todo => {return todo.done === false}).length}}</p>
    <todo v-on:delete-todo="deleteTodo" v-on:complete-todo="completeTodo" v-on:uncomplete-todo="uncompleteTodo" v-for="todo in todos" :todo.sync="todo" :key="todo.id"></todo>
  </div>
</template>

<script type = "text/javascript" >

import Todo from './Todo';

export default {
  props: ['todos'],
  components: {
    Todo,
  },
  mounted() {
    if (JSON.parse(window.localStorage.getItem('todos')) !== null) {
      this.todos = JSON.parse(window.localStorage.getItem('todos'));
      this.$todos = this.todos;
    } else {
      console.log('no localstorage data');
    }
  },
  methods: {
    deleteTodo(todo) {
      const todoIndex = this.todos.indexOf(todo);
      this.todos.splice(todoIndex, 1);
      this.$emit('save-todos');
    },
    completeTodo(todo) {
      const todoIndex = this.todos.indexOf(todo);
      this.todos[todoIndex].done = true;
      this.$emit('save-todos');
    },
    uncompleteTodo(todo) {
      const todoIndex = this.todos.indexOf(todo);
      this.todos[todoIndex].done = false;
      this.$emit('save-todos');
    },
  },
};

</script>
