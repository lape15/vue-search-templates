<script setup>
import { ref, onMounted, computed, watch } from "vue";
import OneTemplate from "./OneTemplate.vue";
import InputBox from "../search/InputBox.vue";
import NotFound from "../search/NotFound.vue";
import DropdownTab from "../search/DropdownTab.vue";
import axios from "axios";

const templates = ref([]);
const loading = ref(false);
const defaultTemps = ref([]);
const searchValue = ref("");
const page = ref(1);
const cardsPerPage = ref(18);
const indexOfLastCard = ref(page.value * cardsPerPage.value);
const tempRef = ref();

const indexOfFirstCard = ref(indexOfLastCard.value - cardsPerPage.value);
async function fetchTemplates() {
  loading.value = true;
  try {
    const data = await axios.get(
      "https://front-end-task-dot-fpls-dev.uc.r.appspot.com/api/v1/public/task_templates"
    );
    loading.value = false;
    defaultTemps.value = [...data.data];
    templates.value = [...data.data];
  } catch (err) {
    console.error(err, "error here");
  }
}
onMounted(() => {
  fetchTemplates();
});

function changePage(newPage) {
  page.value = newPage;
  const temp = document.querySelector(".templates");
  temp.scrollIntoView({
    behavior: "smooth",
  });
}

const handleCreate = (event, param) => {
  if (event !== "" && param === "name") {
    return (templates.value = templates.value.filter(
      (temp) =>
        temp.name.toLowerCase().indexOf(searchValue.value.toLowerCase()) !== -1
    ));
  }
  if (event !== "All" && param === "categories") {
    searchValue.value = "";
    console.log({ event });
    try {
      return (templates.value = defaultTemps.value.filter((template) =>
        template.category.includes(event)
      ));
    } catch (err) {
      console.error(err, "error");
    }
  }
  if (param === "sort" && event === "Descending") {
    return templates.value.sort((a, b) => b.name.localeCompare(a.name));
  } else if (param === "sort" && event === "Ascending") {
    return (templates.value = templates.value.sort((a, b) =>
      a.name.localeCompare(b.name)
    ));
  }

  if (param === "dateSort" && event === "Ascending") {
    return templates.value.sort(
      (a, b) => new Date(a.created).getTime() - new Date(b.created).getTime()
    );
  } else if (param === "dateSort" && event === "Descending") {
    templates.value = templates.value.sort(
      (a, b) => new Date(b.created).getTime() - new Date(a.created).getTime()
    );
  }
  templates.value = defaultTemps.value;
  console.log(defaultTemps.value[0], templates.value[0], "HAHAHAH");
};
watch(page, (newPage) => {
  indexOfLastCard.value = newPage * cardsPerPage.value;
  indexOfFirstCard.value = indexOfLastCard.value - cardsPerPage.value;
});
</script>

<template>
  <div class="search-container">
    <InputBox @search-templates="handleCreate" v-model="searchValue" />
    <DropdownTab @search-templates="handleCreate" />
  </div>

  <p v-if="loading" class="p">Loading....</p>
  <div class="p" v-if="templates.length">{{ templates.length }}Found</div>
  <div class="templates" ref="tempRef">
    <div
      v-for="template in templates.slice(indexOfFirstCard, indexOfLastCard)"
      :key="template.name"
    >
      <OneTemplate :template="template" />
    </div>
    <NotFound v-if="templates.length === 0 && !loading">
      Templates unavailable...
    </NotFound>
  </div>
  <div v-if="templates.length > 0">
    <button @click="changePage(page - 1)">previous</button>
    <button>
      {{ page }}
    </button>
    <span>
      of
      {{ (templates.length / cardsPerPage).toFixed() }}
    </span>
    <button @click="changePage(page + 1)">next</button>
  </div>
</template>

<style scoped>
.p {
  padding: 20px;
  display: flex;
  justify-content: center;
}
.search-container {
  display: flex;
  padding: 20px;
}
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
