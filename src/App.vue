<template>
  <div>
    <count-todos v-bind:todos="todos"></count-todos>
    <download-todos v-on:download-todos="downloadTodos"></download-todos>
    <sort-todos v-bind:todos="todos"></sort-todos>
    <todo-list v-bind:todos="todos" v-on:save-todos="saveTodos"></todo-list>
    <create-todo v-on:add-todo="addTodo" v-on:save-todos="saveTodos"></create-todo>
    <paginate-todos v-bind:todos="todos"></paginate-todos>
  </div>
</template>

<script>
import CountTodos from './components/CountTodos';
import DownloadTodos from './components/DownloadTodos';
import SortTodos from './components/SortTodos';
import TodoList from './components/TodoList';
import CreateTodo from './components/CreateTodo';
import PaginateTodos from './components/PaginateTodos';

export default {
  name: 'app',
  components: {
    CountTodos,
    DownloadTodos,
    SortTodos,
    TodoList,
    CreateTodo,
    PaginateTodos,
  },
  mounted() {
    if (localStorage.getItem('todos_all')) {
      this.localstorageTodos = JSON.parse(localStorage.getItem('todos_all'));
    }
    const url = new URL(window.location.href);
    if (url.searchParams.get('page')) {
      this.currentPage = url.searchParams.get('page');
    }
    if (window.location.href.indexOf('?') < 0) {
      window.location.href = `?page=${this.currentPage}`;
    }
    if (this.localstorageTodos) {
      const todosShowingRangeMax = this.currentPage * 5;
      const todosShowingRangeMin = (this.currentPage * 5) - 5;
      this.todos = this.localstorageTodos.slice(todosShowingRangeMin, todosShowingRangeMax);
    }
  },
  methods: {
    paginateTodos() {
      localStorage.setItem('pagination_todos_quantity', this.todos.length);
    },
    saveTodos() {
      localStorage.setItem('todos_all', JSON.stringify(this.localstorageTodos));
      localStorage.setItem('todos_with_completed', JSON.stringify(this.localstorageTodos));
    },
    addTodo(newtitle, newproject) {
      let uidToExport = 1;
      if (localStorage.getItem('last_uid') !== undefined) {
        uidToExport = JSON.parse(localStorage.getItem('last_uid')) + 1;
      }
      JSON.stringify(localStorage.setItem('last_uid', uidToExport));
      if (localStorage.getItem('todos_all')) {
        this.localstorageTodos = JSON.parse(localStorage.getItem('todos_all'));
      }
      this.localstorageTodos.push({
        uid: uidToExport,
        title: newtitle,
        project: newproject,
        done: false,
      });
      this.saveTodos();
      if (this.currentPage < Math.ceil(this.localstorageTodos.length / 5)) {
        const newPageToSwitch = Math.ceil(this.localstorageTodos.length / 5);
        const searchParams = new URLSearchParams(window.location.search);
        searchParams.set('page', newPageToSwitch);
        window.location.search = searchParams.toString();
      } else {
        document.location.reload();
      }
    },
    downloadTodos() {
      let tmp = JSON.stringify(Object.values(this.localstorageTodos));
      tmp = tmp.substring(2);
      tmp = tmp.slice(0, -2);
      tmp = tmp.replace(/"uid":/g, '');
      tmp = tmp.replace(/"title":/g, '');
      tmp = tmp.replace(/"project":/g, '');
      tmp = tmp.replace(/"done":/g, '');
      tmp = tmp.replace(/},{/g, '\n');
      tmp = tmp.replace(/,/g, ';');
      tmp = tmp.replace(/"/g, '');
      tmp = tmp.replace(/true/g, '"YES"');
      tmp = tmp.replace(/false/g, '"NO"');
      const todosToDownload = `ID;TITLE;PROJECT;DONE\n ${tmp}`;
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
      currentPage: '1',
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
