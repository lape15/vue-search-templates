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
      dateSort: "Default",
      sort: "Default",
      orders: ["Default", "Ascending", "Descending"],
      date: ["Default", "Ascending", "Descending"],
    };
  },
  methods: {
    logChange() {
      this.$emit("search-templates", this.categoryVal, "categories");
    },
    sortByOrder() {
      this.dateSort = "Default";
      this.$emit("search-templates", this.sort, "sort");
    },
    sortByDate() {
      this.sort = "Default";
      this.$emit("search-templates", this.dateSort, "dateSort");
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
    <select v-model="sort" @change="sortByOrder">
      <option v-for="order in orders" :key="order">
        {{ order }}
      </option>
    </select>
    <select v-model="dateSort" @change="sortByDate">
      <option v-for="dateOrder in date" :key="dateOrder">
        {{ dateOrder }}
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
  margin: 0 5px;
}
</style>
