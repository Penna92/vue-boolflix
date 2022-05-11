<template>
  <div id="series-card" class="container d-flex text-center">
    <div>
      <img class="" :src="item.poster" alt="" />
      <h6 class="my-3">{{ item.name }}</h6>
      <p>{{ item.original_name }}</p>
      <p>{{ item.original_language }}</p>
      <p>{{ item.vote_average }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "AppSeriesCard",
  props: ["item"],
};
</script>

<style lang="scss" scoped>
@import "../assets/styles/variables.scss";
#series-card {
  //   width: 200px;
  //   min-width: 200px;
  background-color: black;
  cursor: pointer;
  transition: transform 0.5s;
  &:hover {
    background-color: lighten(black, 10%);
    // filter: invert(0.1);
    transform: scale(1.1);
  }
  div {
    width: 100%;
    padding: 20px 0;
    h6 {
      text-transform: uppercase;
      color: white;
    }
    img {
      box-shadow: 0 0 6px 3px rgba(#000, 0.15);
      width: 100%;
      // height: 100px;
    }
    p {
      margin-bottom: 0;
      //   color: $text-color;
    }
  }
}
</style>