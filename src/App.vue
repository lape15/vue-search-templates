<script setup>
import HelloWorld from "./components/HelloWorld.vue";
import TemplatesView from "./components/templates/TemplatesView.vue";
</script>

<template>
  <div class="wrapper">
    <TemplatesView />
  </div>
</template>

<style>
@import "@/assets/base.css";
* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}
.wrapper {
  width: 100%;
  height: 100vh;
  overflow-y: scroll;
  display: flex;
  flex-direction: column;
}
</style>
