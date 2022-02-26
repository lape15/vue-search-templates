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
    logChange(val) {
      this.$emit("search-templates", val, "name");
    },
  },
};
</script>

<template>
  <div class="search">
    <input
      v-model="searchValue"
      placeholder="Type here"
      @input="
        $emit('update:modelValue', $event.target.value);
        logChange($event.target.value);
      "
    />
    
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
input {
  /* width: 100%;
   */
  flex: auto;
  padding: 8px;
  border-radius: 4px;
  border: 1px solid bisque;
}
</style>
