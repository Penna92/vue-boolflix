<template>
  <header>
    <div class="container d-flex justify-content-between align-items-center">
      <h1>BOOLFLIX</h1>
    </div>
  </header>
</template>

<script>
export default {
  name: "AppHeader",
  data() {
    return {};
  },
  //   methods: {
  //     search() {
  //       this.$emit("search", this.inputText);
  //       //   this.inputText = "";
  //     },
  //   },
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
    input {
      width: 300px !important;
    }
  }
}
</style>