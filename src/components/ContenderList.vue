<template>
  <b-list-group v-show="showContenders">
    <b-list-group-item variant="secondary" class="d-flex justify-content-between" >
      <span class="sm_label">Contender</span>
      <span class="sm_label">Number of Entries</span>
      <span class="sm_label">Delete</span>
    </b-list-group-item>
    <b-list-group-item class="d-flex justify-content-between " v-for="(contender, i) in contenders" v-bind:key="contender.id">
      <span>{{ contender.text }}</span>
      <span>{{ contender.chances }}</span>
      <b-button @click="$emit('delete-contender', i)">X</b-button>
    </b-list-group-item>
  </b-list-group>
</template>


<script>
export default {
  name: "contender-list",
  props: {
    contenders: {
      type: Array,
      required: true,
    },
    showContenders: {
      type: Boolean,
      required: true
    }
  },
};
</script>


<style scoped>
  .sm_label {
    font-size: 0.9rem;
    font-weight: 700;
    font-style: italic;
  }
  </style>