<script setup>
import { ref, onMounted, computed } from "vue";
import OneTemplate from "./OneTemplate.vue";
import InputBox from "../search/InputBox.vue";
import NotFound from "../search/NotFound.vue";
import DropdownTab from "../search/DropdownTab.vue";
import axios from "axios";

const templates = ref([]);
const loading = ref(false);
const defaultTemps = ref([]);
const searchValue = ref("");
async function fetchTemplates() {
  loading.value = true;
  try {
    const data = await axios.get(
      "https://front-end-task-dot-fpls-dev.uc.r.appspot.com/api/v1/public/task_templates"
    );
    loading.value = false;
    templates.value = data.data.slice(1, 20);
    defaultTemps.value = data.data.slice(1, 20);
  } catch (err) {
    console.error(err, "error hear");
  }
}
onMounted(() => {
  fetchTemplates();
});

const handleCreate = (event, param) => {
  if (event !== "" && param === "name") {
    return (templates.value = templates.value.filter(
      (temp) =>
        temp.name.toLowerCase().indexOf(searchValue.value.toLowerCase()) !== -1
    ));
  }
  if (event !== "All" && param === "categories") {
    searchValue.value = "";
    try {
      return (templates.value = defaultTemps.value.filter((template) =>
        template.category.includes(event)
      ));
    } catch (err) {
      console.error(err, "error");
    }
  }
  templates.value = defaultTemps.value;
};
</script>

<template>
  <InputBox @search-templates="handleCreate" v-model="searchValue" />
  <DropdownTab @search-templates="handleCreate" />
  <p v-if="loading">Loading....</p>
  <div class="templates">
    <div v-for="template in templates" :key="template.name">
      <OneTemplate :template="template" />
    </div>
    <NotFound v-if="templates.length === 0 && !loading">
      Templates unavailable...
    </NotFound>
  </div>
</template>

<style scoped>
.templates {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  padding: 20px 40px;
  width: 100%;
  height: max-content;
  align-items: center;
}
</style>
