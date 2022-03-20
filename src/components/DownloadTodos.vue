<template>
  <div>
    <button class='ui basic button icon' @click="openForm" v-if="!isCreating">
      <i class='plus icon'></i>
    </button>
  </div>
</template>

<script>
export default {

  methods: {
    openForm() {
      this.isCreating = true;
    },
  },
};
</script>
