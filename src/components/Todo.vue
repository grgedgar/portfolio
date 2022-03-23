<template>
  <div class='ui centered card'>
    <div class="content" v-if="!isEditing">
      <div class='header'>
          {{ todo.title }}
      </div>
      <div class='meta'>
          {{ todo.project }}
      </div>
      <div class='extra content'>
          <span class='right floated edit icon' @click="showForm">
          <i class='edit icon'></i>
         </span>
        <span class='right floated trash icon' @click="deleteTodo(todo)">
          <i class='trash icon'></i>
        </span>
      </div>
    </div>
    <div class="content" v-if="isEditing">
      <div class='ui form'>
        <div class='field'>
          <label>Title</label>
          <input type='text' v-model="todo.title" />
        </div>
        <div class='field'>
          <label>Project</label>
          <input type='text' v-model="todo.project" />
        </div>
        <div class='ui two button attached buttons'>
          <button class='ui basic blue button' @click="hideForm">
            Close X
          </button>
        </div>
      </div>
    </div>
    <div class='ui bottom attached green basic button' v-if="!isEditing &&todo.done" @click="uncompleteTodo(todo)">
        Completed
    </div>
    <div class='ui bottom attached red basic button' v-if="!isEditing && !todo.done" @click="completeTodo(todo)">
        Pending
    </div>
  </div>
</template>


<script>
export default {
  props: ['todo'],
  data() {
    return {
      isEditing: false,
      localstorageTodos: [],
    };
  },
  methods: {
    showForm() {
      this.isEditing = true;
    },
    hideForm() {
      this.isEditing = false;
      this.$emit('save-todos');
    },
    deleteTodo(todo) {
      this.$emit('delete-todo', todo);

    },
    completeTodo(todo) {
      this.$emit('complete-todo', todo);
    },
    uncompleteTodo(todo) {
      this.$emit('uncomplete-todo', todo);
    },
  },
};
</script>
