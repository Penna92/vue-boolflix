<template>
  <div id="app">
    <app-header @performSearch="search" @resetSearch="reset" />
    <main>
      <section id="main-grid" class="container">
        <div
          v-if="
            movieList.length === 0 &&
            seriesList.length === 0 &&
            pressedSearch === false
          "
          id="initialPage"
        >
          <h2 class="text-white mt-3">I consigliati da BoolFlix</h2>
          <div class="text-center mt-5">
            <h2 class="text-white">Movies</h2>
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
              v-for="media in recommendedMovies"
              :key="media.id"
              class="
                d-flex
                justify-content-center
                col-6 col-sm-6 col-md-4 col-lg-3
                m-3
              "
            >
              <app-movie-card :item="media" />
            </div>
          </div>
          <div class="text-center mt-5">
            <h2 class="text-white">TV series</h2>
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
              v-for="media in recommendedSeries"
              :key="media.id"
              class="
                my-5
                d-flex
                justify-content-center
                col-6 col-sm-6 col-md-4 col-lg-3
                m-3
              "
            >
              <app-series-card :item="media" />
            </div>
          </div>
        </div>
        <div class="text-center mt-5">
          <h2 v-if="movieList.length > 0">Movies</h2>
          <h2
            style="color: red"
            v-if="movieList.length === 0 && pressedSearch === true"
          >
            Nessun film corrisponde alla tua ricerca
          </h2>
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
              col-6 col-sm-6 col-md-4 col-lg-3
              m-3
            "
          >
            <app-movie-card :item="media" />
          </div>
        </div>
        <div class="text-center mt-5">
          <h2 v-if="seriesList.length > 0">TV series</h2>
          <h2
            style="color: red"
            v-if="seriesList.length === 0 && pressedSearch === true"
          >
            Nessuna serie TV corrisponde alla tua ricerca
          </h2>
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
              col-6 col-sm-6 col-md-4 col-lg-3
              m-3
            "
          >
            <app-series-card :item="media" />
          </div>
        </div>
      </section>
    </main>
  </div>
</template>

<script>
import axios from "axios";
import AppHeader from "./components/AppHeader.vue";
import AppMovieCard from "./components/AppMovieCard.vue";
import AppSeriesCard from "./components/AppSeriesCard.vue";
export default {
  name: "App",
  components: {
    AppHeader,
    AppMovieCard,
    AppSeriesCard,
  },
  data() {
    return {
      // cast: [],
      recommendedMovies: [],
      recommendedSeries: [],
      movieList: [],
      seriesList: [],
      apiUrl: "https://api.themoviedb.org/3/search/",
      apiKey: "56b444989b81740766d743a8aa50b267",
      pressedSearch: false,
    };
  },
  methods: {
    reset(text) {
      (this.seriesList = []), (this.movieList = []), console.log(text);
      this.pressedSearch = false;
    },
    search(text) {
      this.pressedSearch = true;
      const paramsObj = {
        params: {
          api_key: this.apiKey,
          query: text,
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
          (this.seriesList = []), (this.movieList = []), (this.loading = false);
        });
    },
  },
  mounted() {
    axios
      .get(
        "https://api.themoviedb.org/3/search/movie?api_key=56b444989b81740766d743a8aa50b267&query=ritorno"
      )
      .then((res) => {
        this.recommendedMovies = res.data.results;
      })
      .catch((error) => {
        console.log(error);
        this.loading = false;
      });
    axios
      .get(
        "https://api.themoviedb.org/3/search/tv?api_key=56b444989b81740766d743a8aa50b267&query=star"
      )
      .then((res) => {
        this.recommendedSeries = res.data.results;
      })
      .catch((error) => {
        console.log(error);
        (this.recommendedSeries = []),
          (this.movieList = []),
          (this.loading = false);
      });
  },
};
</script>

<style lang="scss">
@import "./assets/styles/general.scss";
h2 {
  color: white;
}
</style>
