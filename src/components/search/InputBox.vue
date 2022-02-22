<script>
export default {
  props: {
    methods: { type: Function },
    searchVal: String,
  },
  data() {
    return {
      searchValue: "",
    };
  },
  methods: {
    logChange() {
      this.$emit("search-templates", this.searchValue);
    },
  },
};
</script>

<template>
  <div class="search">
    <input v-model="searchValue" placeholder="Type here" @input="logChange" />
  </div>
</template>

<style scoped>
.search {
  width: 100%;
  display: flex;
  flex-wrap: nowrap;
  padding: 20px;
}
input {
  /* width: 100%;
   */
  flex: auto;
  padding: 8px;
  border-radius: 4px;
  border: 1px solid bisque;
}
</style>
