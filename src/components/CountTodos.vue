<template>
  <div>
    <p>Completed Tasks: {{todos.filter(todo => {return todo.done === true}).length}}</p>
    <p>Pending Tasks: {{todos.filter(todo => {return todo.done === false}).length}}</p>
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
};

</script>
