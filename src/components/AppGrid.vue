<template>
  <section id="main-grid" class="container">
    <div class="container d-flex">
      <input
        type="text"
        class="form-control"
        placeholder="Cerca film o serie tv"
        aria-label="Cerca film o serie tv"
        v-model="inputText"
        @keyup.enter="filterMedia"
      />
      <button
        @click="filterMedia"
        class="btn btn-outline-secondary"
        type="button"
      >
        Search
      </button>
      <button @click="reset" class="btn btn-outline-secondary" type="button">
        Reset
      </button>
    </div>

    <div class="text-center mt-5">
      <h2 v-if="movieList.length > 0">Film</h2>
    </div>

    <div
      class="
        container
        d-flex
        flex-wrap
        justify-content-center
        align-items-center
      "
    >
      <div
        v-for="media in movieList"
        :key="media.id"
        class="
          d-flex
          justify-content-center
          col-6 col-sm-4 col-md-3 col-lg-2
          m-3
        "
      >
        <app-movie-card :item="media" />
      </div>
    </div>
    <div class="text-center mt-5">
      <h2 v-if="seriesList.length > 0">Serie TV</h2>
    </div>
    <div
      class="
        container
        d-flex
        flex-wrap
        justify-content-center
        align-items-center
      "
    >
      <div
        v-for="media in seriesList"
        :key="media.id"
        class="
          my-5
          d-flex
          justify-content-center
          col-6 col-sm-4 col-md-3 col-lg-2
          m-3
        "
      >
        <app-series-card :item="media" />
      </div>
    </div>
  </section>
</template>

<script>
// import store from "../store.js";
import axios from "axios";
import AppMovieCard from "./AppMovieCard.vue";
import AppSeriesCard from "./AppSeriesCard.vue";
export default {
  name: "AppGrid",
  components: {
    AppMovieCard,
    AppSeriesCard,
  },
  // props: {
  //   msg: String,
  // },
  data() {
    return {
      inputText: "",
      movieList: [],
      seriesList: [],
      apiUrl: "https://api.themoviedb.org/3/search/",
      apiKey: "56b444989b81740766d743a8aa50b267",
    };
  },
  methods: {
    filterMedia() {
      const paramsObj = {
        params: {
          api_key: this.apiKey,
          query: this.inputText,
          // language: "it-IT",
        },
      };
      axios
        .get(this.apiUrl + "movie", paramsObj)
        .then((res) => {
          console.log(res);
          this.movieList = res.data.results;
        })
        .catch((error) => {
          console.log(error);
          this.loading = false;
        });
      axios
        .get(this.apiUrl + "tv", paramsObj)
        .then((res) => {
          console.log(res);
          this.seriesList = res.data.results;
        })
        .catch((error) => {
          console.log(error);
          this.loading = false;
        });
    },
    reset() {
      this.inputText = "";
    },
  },
  mounted() {},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h2 {
  color: white;
}
</style>
