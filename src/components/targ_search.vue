<template>
    <v-text-field
      v-model="searchQuery"
      label="Поиск"
      clearable
      @input="applyFilter"
    />
  </template>
  
  <script>
  export default {
    name: 'targ_search',
    data() {
      return {
        searchQuery: ''
      };
    },
    methods: {
      applyFilter() {
        this.$emit('filter-changed', this.searchQuery);
      }
    }
  };
  </script>
  
  <style scoped>
  /* Стили для компонента поиска, если необходимо */
  </style>
  