<template>
  <div>
    <count-todos v-bind:todos="todos"></count-todos>
    <download-todos v-on:download-todos="downloadTodos"></download-todos>
    <todo-list v-bind:todos="todos" v-on:save-todos="saveTodos"></todo-list>
    <create-todo v-on:add-todo="addTodo" v-on:save-todos="saveTodos"></create-todo>
    <paginate-todos v-bind:todos="todos"></paginate-todos>
  </div>
</template>

<script>
import CountTodos from './components/CountTodos';
import DownloadTodos from './components/DownloadTodos';
import TodoList from './components/TodoList';
import CreateTodo from './components/CreateTodo';
import PaginateTodos from './components/PaginateTodos';

export default {
  name: 'app',
  components: {
    CountTodos,
    DownloadTodos,
    TodoList,
    CreateTodo,
    PaginateTodos,
  },
  mounted() {
    if (localStorage.getItem('todos_all')) {
      this.localstorageTodos = JSON.parse(localStorage.getItem('todos_all'));
    }
    let pageNumber = 1;
    if (window.location.href.indexOf('?') > -1) {
      pageNumber = window.location.href.split('=')[1];
    }
    if (window.location.href.indexOf('?') < 0) {
      window.location.href = `?page=${pageNumber}`;
    }
    if (this.localstorageTodos) {
      const todosShowingRangeMax = pageNumber * 5;
      const todosShowingRangeMin = (pageNumber * 5) - 5;
      this.todos = this.localstorageTodos.slice(todosShowingRangeMin, todosShowingRangeMax);
    }
  },
  methods: {
    paginateTodos() {
      localStorage.setItem('pagination_todos_quantity', this.todos.length);
    },
    saveTodos() {
      localStorage.setItem('todos_all', JSON.stringify(this.localstorageTodos));
    },
    addTodo(newtitle, newproject) {
      if (localStorage.getItem('todos_all')) {
        this.localstorageTodos = JSON.parse(localStorage.getItem('todos_all'));
      }
      this.localstorageTodos.push({
        title: newtitle,
        project: newproject,
        done: false,
      });
      document.location.reload();
    },
    downloadTodos() {
      let tmp = JSON.stringify(Object.values(this.localstorageTodos));
      tmp = tmp.substring(2);
      tmp = tmp.slice(0, -2);
      tmp = tmp.replace(/"title":/g, '');
      tmp = tmp.replace(/"project":/g, '');
      tmp = tmp.replace(/"done":/g, '');
      tmp = tmp.replace(/},{/g, '\n');
      tmp = tmp.replace(/,/g, ';');
      tmp = tmp.replace(/"/g, '');
      tmp = tmp.replace(/true/g, '"YES"');
      tmp = tmp.replace(/false/g, '"NO"');
      const todosToDownload = `TITLE;PROJECT;DONE\n ${tmp}`;
      const dataStr = `data:application/vnd.ms-excel;charset=utf-8,${encodeURIComponent(todosToDownload)}`;
      const downloadAnchorNode = document.createElement('a');
      downloadAnchorNode.setAttribute('href', dataStr);
      downloadAnchorNode.setAttribute('download', 'todos.csv');
      document.body.appendChild(downloadAnchorNode);
      downloadAnchorNode.click();
      downloadAnchorNode.remove();
    },
  },
  data() {
    return {
      todos: [],
      localstorageTodos: [],
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
