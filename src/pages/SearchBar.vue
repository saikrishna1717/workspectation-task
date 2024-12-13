<template>
<v-text-field v-model="searchQuery" label="Search users by name or company" clearable @input="emitSearch" />
</template>

  
<script>
export default {
    name: 'SearchBar',
    data() {
        return {
            searchQuery: '',
        };
    },
    methods: {
        emitSearch() {
            this.$emit('search', this.searchQuery);
        },
    },
};
</script>

  
<style scoped>
  </style>
