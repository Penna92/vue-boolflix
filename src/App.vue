<template>
  <div id="app">
    <app-header @performSearch="search" @resetSearch="reset" />
    <main>
      <section id="main-grid" class="container">
        <app-loader v-if="loading" />
        <app-select-genre
          v-if="!loading"
          @searchGenre="setSearchGenre($event)"
          :mediaGenres="genres"
        />
        <div
          v-if="
            movieList.length === 0 &&
            seriesList.length === 0 &&
            pressedSearch === false &&
            loading === false
          "
          id="initialPage"
        >
          <h3 class="text-white mt-3">I consigliati da BoolFlix</h3>
          <div class="text-center mt-3">
            <h3 v-if="filteredRecommendedMovies.length > 0" class="text-white">
              Film
            </h3>
            <h3
              style="color: red"
              v-if="filteredRecommendedMovies.length === 0"
            >
              Nessun film corrisponde alla tua ricerca
            </h3>
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
              v-for="media in filteredRecommendedMovies"
              :key="media.id"
              class="m-5 d-flex justify-content-center"
            >
              <app-movie-card :item="media" />
            </div>
          </div>
          <div class="text-center mt-3">
            <h3 v-if="filteredRecommendedSeries.length > 0" class="text-white">
              Serie TV
            </h3>
            <h3
              style="color: red"
              v-if="filteredRecommendedSeries.length === 0"
            >
              Nessuna serie TV corrisponde alla tua ricerca
            </h3>
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
              v-for="media in filteredRecommendedSeries"
              :key="media.id"
              class="m-5 d-flex justify-content-center"
            >
              <app-series-card :item="media" />
            </div>
          </div>
        </div>
        <div class="text-center mt-3">
          <h3 v-if="filteredMovies.length > 0">Film</h3>
          <h3
            style="color: red"
            v-if="filteredMovies.length === 0 && pressedSearch === true"
          >
            Nessun film corrisponde alla tua ricerca
          </h3>
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
            v-for="media in filteredMovies"
            :key="media.id"
            class="d-flex justify-content-center m-5"
          >
            <app-movie-card :item="media" :filteredGenre="searchText" />
          </div>
        </div>
        <div class="text-center mt-3">
          <h3 v-if="filteredSeries.length > 0">Serie TV</h3>
          <h3
            style="color: red"
            v-if="filteredSeries.length === 0 && pressedSearch === true"
          >
            Nessuna serie TV corrisponde alla tua ricerca
          </h3>
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
            v-for="media in filteredSeries"
            :key="media.id"
            class="d-flex justify-content-center m-5"
          >
            <app-series-card :item="media" :filteredGenre="searchText" />
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
import AppSelectGenre from "./components/AppSelectGenre.vue";
import AppLoader from "./components/AppLoader.vue";
export default {
  name: "App",
  components: {
    AppHeader,
    AppMovieCard,
    AppSeriesCard,
    AppSelectGenre,
    AppLoader,
  },
  data() {
    return {
      recommendedMovies: [],
      recommendedSeries: [],
      movieList: [],
      seriesList: [],
      apiUrl: "https://api.themoviedb.org/3/search/",
      apiKey: "56b444989b81740766d743a8aa50b267",
      pressedSearch: false,
      loading: false,
      genres: [
        {
          id: 28,
          name: "Action",
        },
        {
          id: 12,
          name: "Adventure",
        },
        {
          id: 16,
          name: "Animation",
        },
        {
          id: 35,
          name: "Comedy",
        },
        {
          id: 80,
          name: "Crime",
        },
        {
          id: 99,
          name: "Documentary",
        },
        {
          id: 18,
          name: "Drama",
        },
        {
          id: 10751,
          name: "Family",
        },
        {
          id: 14,
          name: "Fantasy",
        },
        {
          id: 36,
          name: "History",
        },
        {
          id: 27,
          name: "Horror",
        },
        {
          id: 10402,
          name: "Music",
        },
        {
          id: 9648,
          name: "Mystery",
        },
        {
          id: 10749,
          name: "Romance",
        },
        {
          id: 878,
          name: "Science Fiction",
        },
        {
          id: 10770,
          name: "TV Movie",
        },
        {
          id: 53,
          name: "Thriller",
        },
        {
          id: 10752,
          name: "War",
        },
        {
          id: 37,
          name: "Western",
        },
        {
          id: 10759,
          name: "Action & Adventure",
        },
        {
          id: 10762,
          name: "Kids",
        },
        {
          id: 10763,
          name: "News",
        },
        {
          id: 10764,
          name: "Reality",
        },
        {
          id: 10765,
          name: "Sci-Fi & Fantasy",
        },
        {
          id: 10766,
          name: "Soap",
        },
        {
          id: 10767,
          name: "Talk",
        },
        {
          id: 10768,
          name: "War & Politics",
        },
      ],
      searchText: undefined,
      searchId: undefined,
    };
  },
  methods: {
    setSearchGenre(text) {
      this.searchText = text.name;
      this.searchId = text.id;
      // console.log(this.searchId);
      // console.log(this.searchText);
      // console.log(text, this.searchText);
    },
    reset(text) {
      (this.seriesList = []), (this.movieList = []), console.log(text);
      this.pressedSearch = false;
      this.searchId = undefined;
      // this.searchText = "";
    },
    search(text) {
      this.pressedSearch = true;
      this.loading = true;
      const paramsObj = {
        params: {
          api_key: this.apiKey,
          query: text,
        },
      };
      axios
        .get(this.apiUrl + "movie", paramsObj)
        .then((res) => {
          // console.log(res);
          this.movieList = res.data.results;
          this.loading = false;
        })
        .catch((error) => {
          console.log(error);
          this.loading = false;
        });
      axios
        .get(this.apiUrl + "tv", paramsObj)
        .then((res) => {
          // console.log(res);
          this.seriesList = res.data.results;
          this.loading = false;
        })
        .catch((error) => {
          console.log(error);
          (this.seriesList = []), (this.movieList = []), (this.loading = false);
        });
    },
  },
  mounted() {
    this.loading = true;
    setTimeout(() => {
      axios
        .get(
          "https://api.themoviedb.org/3/search/movie?api_key=56b444989b81740766d743a8aa50b267&query=ritorno"
        )
        .then((res) => {
          this.recommendedMovies = res.data.results;
          this.loading = false;
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
    }, 2000);
  },
  computed: {
    filteredRecommendedMovies() {
      if (this.searchId === undefined) {
        return this.recommendedMovies;
      }
      return this.recommendedMovies.filter((el) => {
        if (el.genre_ids.includes(this.searchId)) {
          // console.log(el.genre_ids[0], this.searchId, el);
          return el;
        }
      });
    },
    filteredRecommendedSeries() {
      if (this.searchId === undefined) {
        return this.recommendedSeries;
      }
      return this.recommendedSeries.filter((el) => {
        if (el.genre_ids.includes(this.searchId)) {
          // console.log(el.genre_ids[0], this.searchId, el);
          return el;
        }
      });
    },
    filteredMovies() {
      if (this.searchId === undefined) {
        return this.movieList;
      }
      return this.movieList.filter((el) => {
        if (el.genre_ids.includes(this.searchId)) {
          // console.log(el.genre_ids[0], this.searchId, el);
          return el;
        }
      });
    },
    filteredSeries() {
      if (this.searchId === undefined) {
        return this.seriesList;
      }
      return this.seriesList.filter((el) => {
        if (el.genre_ids.includes(this.searchId)) {
          // console.log(el.genre_ids[0], this.searchId, el);
          return el;
        }
      });
    },
  },
};
</script>

<style lang="scss">
@import "./assets/styles/general.scss";
h3 {
  color: white;
}
</style>
