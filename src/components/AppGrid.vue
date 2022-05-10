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
      <div
        v-for="media in seriesList"
        :key="media.id"
        class="
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
      // filteredMediaList: [],
      inputText: "",
      movieList: [],
      seriesList: [],
      // movies: [],
      // series: [],
      // searchText: "",
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
          // this.movieList.forEach((el) => {
          //   if (!this.filteredmovieList.includes(el.species)) {
          //     this.species.push(el.species);
          //   }
          // });
          // store.setSpecies(this.species);
          // console.log(this.characterList);
          // this.loading = false;
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
          // this.filteredList.forEach((el) => {
          //   if (!this.filteredfilteredList.includes(el.species)) {
          //     this.species.push(el.species);
          //   }
          // });
          // store.setSpecies(this.species);
          // console.log(this.characterList);
          // this.loading = false;
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
</style>
