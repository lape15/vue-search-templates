<script>
// const categories = ["Health", "E-commerce", "Education"];
export default {
  props: {
    methods: { type: Function },
    searchVal: String,
  },
  data() {
    return {
      categoryVal: "All",
      categories: ["All", "Health", "E-commerce", "Education"],
    };
  },
  methods: {
    logChange() {
      this.$emit("search-templates", this.categoryVal, "categories");
    },
  },
};
</script>

<template>
  <div class="search">
    <select v-model="categoryVal" @change="logChange">
      <option v-for="category in categories" :key="category">
        {{ category }}
      </option>
    </select>
  </div>
</template>

<style scoped>
.search {
  width: 100%;
  display: flex;
  flex-wrap: nowrap;
  padding: 20px;
  margin: 0 auto;
}
select {
  /* width: 100%;
   */
  flex: auto;
  padding: 8px;
  border-radius: 4px;
  border: 1px solid bisque;
}
</style>
