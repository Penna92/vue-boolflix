<template>
  <header>
    <div class="container d-flex justify-content-between align-items-center">
      <h1>BOOLFLIX</h1>
      <div class="d-flex align-items-center gap-2">
        <input
          type="text"
          class="form-control"
          placeholder="Cerca film o serie tv"
          aria-label="Cerca film o serie tv"
          v-model="search"
          @keyup.enter="$emit('performSearch', search)"
        />
        <button
          @click="searchMedia"
          class="btn btn-outline-secondary"
          type="button"
        >
          Search
        </button>
        <button @click="reset" class="btn btn-outline-secondary" type="button">
          Reset
        </button>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: "AppHeader",
  data() {
    return {
      search: "",
    };
  },
  methods: {
    searchMedia() {
      this.$emit("performSearch", this.search);
      //   this.search = "";
    },
    reset() {
      this.$emit("resetSearch", this.search);
      this.search = "";
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../assets/styles/general.scss";
header {
  height: 80px;
  width: 100%;
  background-color: black;
  div {
    height: 100%;
    line-height: 80px;
    H1 {
      color: red;
    }
  }
}
</style>