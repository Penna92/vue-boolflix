<template>
  <div
    @mouseover="hover = false"
    @mouseleave="hover = true"
    id="movie-card"
    :class="{ container: !hover }"
    class="card d-flex flex-column justify-content-between text-center"
  >
    <div v-if="hover === true" id="card-image">
      <img
        id="default-image"
        v-if="item.poster_path === null"
        src="https://image.tmdb.org/t/p/w342/wwemzKWzjKYJFfCeiB57q3r4Bcm.png"
        :alt="item.title"
      />
      <img
        v-if="item.poster_path !== null"
        id="poster"
        :src="imgUrl + item.poster_path"
        :alt="item.title"
      />
    </div>
    <div
      :class="{ 'd-none': hover }"
      class="d-flex flex-column align-items-center my-3"
    >
      <p>
        Titolo: <span>{{ item.title }}</span>
      </p>
      <p v-if="item.original_title !== item.title">
        Titolo originale: <span>{{ item.original_title }}</span>
      </p>
      <p
        v-if="
          item.original_language !== 'it' &&
          item.original_language !== 'en' &&
          item.original_language !== 'fr' &&
          item.original_language !== 'es' &&
          item.original_language !== 'de'
        "
      >
        Lingua originale: <span>{{ item.original_language }}</span>
      </p>
      <span
        v-if="
          item.original_language === 'it' ||
          item.original_language === 'en' ||
          item.original_language === 'fr' ||
          item.original_language === 'es' ||
          item.original_language === 'de'
        "
        class="d-flex align-items-center"
      >
        <p class="d-inline">Lingua originale:</p>
        <img
          id="flag"
          class="mb-3"
          :src="require('../assets/img/' + item.original_language + '.png')"
          :alt="item.original_language"
        />
      </span>
      <!-- <p v-if="parseInt(item.vote_average / 2) === 0">Voto: ☆ ☆ ☆ ☆ ☆</p>
      <p v-if="parseInt(item.vote_average / 2) === 1">Voto: ⭐☆ ☆ ☆ ☆</p>
      <p v-if="parseInt(item.vote_average / 2) === 2">Voto: ⭐⭐☆ ☆ ☆</p>
      <p v-if="parseInt(item.vote_average / 2) === 3">Voto: ⭐⭐⭐☆ ☆</p>
      <p v-if="parseInt(item.vote_average / 2) === 4">Voto: ⭐⭐⭐⭐☆</p>
      <p v-if="parseInt(item.vote_average / 2) === 5">Voto: ⭐⭐⭐⭐⭐</p> -->
      <span class="d-flex flex-row gap-1">
        <p v-for="(n, index) in 5" :key="index.id">
          <i
            :class="
              n <= transformVote
                ? 'fa-solid fa-star yellow-star'
                : 'fa-solid fa-star white-star'
            "
          ></i>
        </p>
      </span>
      <p v-if="item.overview !== ''">
        Overview: <span>{{ item.overview }}</span>
      </p>
      <p v-if="cast.length > 0">
        Cast: <br />
        <span v-for="(actors, index) in cast" :key="index">
          {{ actors.name }} <br />
        </span>
      </p>
      <p v-for="(item, index) in filteredGenres" :key="index">
        Genere {{ index + 1 }}:
        <span>{{ filteredGenres[index].name }}</span>
      </p>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "AppMovieCard",
  data() {
    return {
      imgUrl: "https://image.tmdb.org/t/p/w342",
      hover: true,
      cast: [],
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
      ],
    };
  },
  props: ["item", "filteredGenre"],
  mounted() {
    axios
      .get(
        "https://api.themoviedb.org/3/movie/" +
          this.item.id +
          "/credits?api_key=56b444989b81740766d743a8aa50b267"
      )
      .then((res) => {
        this.cast = res.data.cast;
        if (this.cast.length >= 5) {
          this.cast.length = 5;
        }
      });
  },
  computed: {
    filteredGenres() {
      return this.genres.filter((el) => {
        if (this.item.genre_ids.includes(el.id)) {
          // console.log(el.name);
          // this.mediaGenre = el.name;
          return el;
        }
      });
    },
    transformVote() {
      return parseInt(this.item.vote_average / 2);
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../assets/styles/variables.scss";
#movie-card {
  background-color: black;
  height: 400px;
  width: 300px;
  cursor: pointer;
  transition: transform 0.5s;
  &:hover {
    transform: scale(1.2);
    z-index: 999;
  }
  #card-image {
    width: 100%;
    height: 100%;
    #default-image {
      width: 100%;
      height: 100%;
      object-fit: contain;
      object-position: center;
    }
    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      object-position: center;
    }
  }
  div {
    overflow: auto;
    p {
      font-size: 18px;
      color: white;
      font-weight: bold;
      span {
        font-size: 16px;
        font-weight: 300;
        color: lightgray;
      }
    }
    #flag {
      margin-left: 5px;
      width: 30px;
      height: 20px;
    }
  }
  /* width */
  ::-webkit-scrollbar {
    width: 5px;
  }

  /* Track */
  ::-webkit-scrollbar-track {
    background: #f1f1f1;
  }

  /* Handle */
  ::-webkit-scrollbar-thumb {
    background: #888;
  }

  /* Handle on hover */
  ::-webkit-scrollbar-thumb:hover {
    background: #555;
  }
  .yellow-star {
    color: yellow;
  }
  .white-star {
    color: lightgray;
  }
}
</style>