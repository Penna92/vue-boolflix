<template>
  <div id="app">
    <app-header />
    <main>
      <app-grid />
    </main>
  </div>
</template>

<script>
import AppGrid from "./components/AppGrid.vue";
import AppHeader from "./components/AppHeader.vue";

export default {
  name: "App",
  components: {
    AppGrid,
    AppHeader,
  },
};
</script>

<style lang="scss">
@import "./assets/styles/general.scss";
</style>
