<template>
  <div id="app">
    <app-header @performSearch="search" @resetSearch="reset" />
    <main>
      <section id="main-grid" class="container">
        <app-select-genre
          @searchGenre="setSearchGenre($event)"
          :mediaGenres="genres"
        />
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
              class="my-5 mx-3 d-flex justify-content-center"
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
              class="my-5 mx-3 d-flex justify-content-center"
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
            class="d-flex justify-content-center my-5 mx-3"
          >
            <app-movie-card :item="media" :filteredGenre="searchText" />
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
            class="d-flex justify-content-center my-5 mx-3"
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
export default {
  name: "App",
  components: {
    AppHeader,
    AppMovieCard,
    AppSeriesCard,
    AppSelectGenre,
  },
  data() {
    return {
      recommendedMovies: [],
      recommendedSeries: [],
      // filteredMovieList: [],
      // filteredSeriesList: [],
      movieList: [],
      seriesList: [],
      apiUrl: "https://api.themoviedb.org/3/search/",
      apiKey: "56b444989b81740766d743a8aa50b267",
      pressedSearch: false,
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
      searchId: 35,
    };
  },
  methods: {
    setSearchGenre(text) {
      this.searchText = text.name;
      this.searchId = text.id;
      console.log(this.searchId);
      console.log(this.searchText);
      // console.log(text, this.searchText);
    },
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
          // console.log(res);
          this.movieList = res.data.results;
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
        console.log(res);
        console.log(res.data.results);
        console.log(res.data.results[0].genre_ids[0]);
        this.recommendedMovies = res.data.results;
        // res.data.results.forEach((el) => {
        // if (el.genre_ids[0] === this.searchId) {
        //   console.log(el.genre_ids[0], this.searchId);
        //   this.recommendedMovies.push(el);
        //   //     console.log(this.searchId);
        // }
        // });
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
  computed: {
    // filteredMovieList() {
    // if (this.searchText === "") {
    // return this.recommendedMovies;
    // }
    // return this.movieList.filter((el) => {
    // if (this.searchText === ) {
    // return
    //     return el.genre === this.searchText || el.author === this.searchText2;
    //   } else if (this.searchText !== "" && this.searchText2 === "") {
    //     return el.genre === this.searchText || el.author === this.searchText2;
    //   } else {
    //     return el.genre === this.searchText && el.author === this.searchText2;
    //   }
    // });
    // },
  },
};
</script>

<style lang="scss">
@import "./assets/styles/general.scss";
h2 {
  color: white;
}
</style>
