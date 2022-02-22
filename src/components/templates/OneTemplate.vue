<script>
export default {
  props: {
    template: Object,
  },
};
</script>

<template>
  <div class="template">
    <h3>{{ template.name }}</h3>
    <p>{{ template.description }}</p>
  </div>
</template>
<style scoped>
.template {
  width: 300px;
  height: 300px;
  background: white;
  padding: 10px 20px;
  margin: 20px 30px;
  color: #000;
  border-radius: 4px;
}
h3 {
  font-size: 1.2rem;
  color: green;
}
p {
  color: #333;
  font-size: 1rem;
}
</style>
