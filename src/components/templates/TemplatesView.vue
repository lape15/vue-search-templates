<script setup>
import { ref, onMounted, computed } from "vue";
import OneTemplate from "./OneTemplate.vue";
import InputBox from "../search/InputBox.vue";
import axios from "axios";

const templates = ref([]);
const loading = ref(false);
const defaultTemps = ref([]);
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

const handleCreate = (event, searchVal) => {
  if (event !== "") {
    return (templates.value = templates.value.filter(
      (temp) => temp.name.toLowerCase().indexOf(event.toLowerCase()) !== -1
    ));
  }
  templates.value = defaultTemps.value;
};
</script>

<template>
  <InputBox @search-templates="handleCreate" />
  <p v-if="loading">Loading....</p>
  <div class="templates">
    <div v-for="template in templates" :key="template.name">
      <OneTemplate :template="template" />
    </div>
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
