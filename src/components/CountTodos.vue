<template>
  <div>
    <p>Completed Tasks: {{localstorageTodos.filter(todo => {return todo.done === true}).length}}</p>
    <p>Pending Tasks: {{localstorageTodos.filter(todo => {return todo.done === false}).length}}</p>
  </div>
</template>

<script type = "text/javascript" >

import Paginate from 'vuejs-paginate';
import Todo from './Todo';

export default {
  props: ['todos'],
  components: {
    Paginate,
    Todo,
  },
  mounted() {
    if (localStorage.getItem('todos_all')) {
      this.localstorageTodos = JSON.parse(localStorage.getItem('todos_all'));
    }
  },
  data() {
    return {
      localstorageTodos: [],
    };
  },
};

</script>
