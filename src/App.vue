<template>
  <div>
    <todo-list v-bind:todos="todos" v-on:save-todos="saveTodos"></todo-list>
    <create-todo v-on:add-todo="addTodo" v-on:save-todos="saveTodos"></create-todo>
  </div>
</template>

<script>
import DowoloadTodos from './components/DownloadTodos';
import TodoList from './components/TodoList';
import CreateTodo from './components/CreateTodo';

export default {
  name: 'app',
  components: {
    DowoloadTodos,
    TodoList,
    CreateTodo,
  },
  mounted() {
    if (JSON.parse(window.localStorage.getItem('todos')) !== null) {
      this.todos = JSON.parse(window.localStorage.getItem('todos'));
    } else {
      console.log('no localstorage data');
    }
  },
  methods: {
    addTodo(newtitle, newproject) {
      this.todos.push({
        title: newtitle,
        project: newproject,
        done: false,
      });
    },
    saveTodos() {
      const todosToExport = JSON.stringify(this.todos);
      window.localStorage.setItem('todos', todosToExport);
    },
  },
  data() {
    return {
      todos: [],
    };
  },
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
