<template>
  <div class='ui basic content left aligned segment'>
    <button class='ui basic button icon' @click="downloadTodos" >
      <i class='download icon'></i>
    </button>
  </div>
</template>

<script>

export default {
  methods: {
    downloadTodos() {
      this.$emit('download-todos');
    },
  },
};
</script>
