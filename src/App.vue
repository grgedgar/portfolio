<template>
  <div>
    <download-todos v-on:download-todos="downloadTodos"></download-todos>
    <todo-list v-bind:todos="todos" v-on:save-todos="saveTodos"></todo-list>
    <create-todo v-on:add-todo="addTodo" v-on:save-todos="saveTodos"></create-todo>
  </div>
</template>

<script>
import DownloadTodos from './components/DownloadTodos';
import TodoList from './components/TodoList';
import CreateTodo from './components/CreateTodo';

export default {
  name: 'app',
  components: {
    DownloadTodos,
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
    downloadTodos() {
      let tmp = JSON.stringify(Object.values(this.todos));
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
