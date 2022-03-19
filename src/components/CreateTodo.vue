<template>
  <div class='ui basic content center aligned segment'>
    <button class='ui basic button icon' @click="openForm" v-if="!isCreating">
      <i class='plus icon'></i>
    </button>
    <div class='ui centered card' v-if="isCreating">
      <div class='content'>
        <div class='ui form'>
          <div class='field'>
            <label>Title</label>
            <input v-model="titleText" type='text' ref='title' defaultValue="" @change="validateForm" />
          </div>
          <div class='field'>
            <label>Project</label>
            <input v-model="projectText" type='text' ref='project' defaultValue="" @change="validateForm" />
          </div>
          <div class='ui two button attached buttons'>
            <button class='ui basic blue button' @click="sendForm" :disabled="isDisabled">
              Create
            </button>
            <button class='ui basic red button' @click="closeForm">
              Cancel
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
/* import { saveTodos } from './TodoList'; */

export default {
  data() {
    return {
      titleText: '',
      projectText: '',
      isCreating: false,
      buttonDisabled: true,
    };
  },
  computed: {
    isDisabled() {
      return this.buttonDisabled === true;
    },
  },
  methods: {
    openForm() {
      this.isCreating = true;
    },
    closeForm() {
      this.isCreating = false;
    },
    validateForm() {
      if (this.isCreating === true && this.titleText.length > 0 && this.projectText.length > 0) {
        this.buttonDisabled = false;
      } else {
        this.buttonDisabled = true;
      }
    },
    sendForm() {
      if (this.titleText.length > 0 && this.projectText.length > 0) {
        const title = this.titleText;
        const project = this.projectText;
        this.$emit('add-todo', title, project);
        this.titleText = '';
        this.projectText = '';
        this.$emit('save-todos');
      }
      this.isCreating = false;
    },
  },
};
</script>
