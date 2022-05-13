<template>
  <div
    v-if="filteredGenre === filteredGenres || filteredGenre === undefined"
    @mouseover="hover = false"
    @mouseleave="hover = true"
    id="series-card"
    :class="{ container: !hover }"
    class="card d-flex flex-column justify-content-between text-center"
  >
    <div v-if="hover === true" id="card-image">
      <img
        v-if="item.poster_path === null"
        id="default-image"
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
        Titolo: <span>{{ item.name }}</span>
      </p>
      <p v-if="item.original_name !== item.name">
        Titolo originale: <span>{{ item.original_name }}</span>
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
        Lingua originale: {{ item.original_language }}
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

      <p v-if="parseInt(item.vote_average / 2) === 0">Voto: ☆ ☆ ☆ ☆ ☆</p>
      <p v-if="parseInt(item.vote_average / 2) === 1">Voto: ⭐☆ ☆ ☆ ☆</p>
      <p v-if="parseInt(item.vote_average / 2) === 2">Voto: ⭐⭐☆ ☆ ☆</p>
      <p v-if="parseInt(item.vote_average / 2) === 3">Voto: ⭐⭐⭐☆ ☆</p>
      <p v-if="parseInt(item.vote_average / 2) === 4">Voto: ⭐⭐⭐⭐☆</p>
      <p v-if="parseInt(item.vote_average / 2) === 5">Voto: ⭐⭐⭐⭐⭐</p>
      <p v-if="item.overview !== ''">
        Overview: <span>{{ item.overview }}</span>
      </p>
      <p v-if="cast.length > 0">
        Cast : <br />
        <span v-for="(actors, index) in cast" :key="index">
          {{ actors.name }} <br />
        </span>
      </p>
      <p v-if="item.genre_ids.length === 0">
        Genere:
        <span> Not available</span>
      </p>
      <p v-if="item.genre_ids.length !== 0">
        Genere:
        <span>{{ filteredGenres }}</span>
      </p>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "AppSeriesCard",
  props: {
    item: Object,
    filteredGenre: undefined,
  },
  data() {
    return {
      imgUrl: "https://image.tmdb.org/t/p/w342",
      hover: true,
      cast: [],
      genres: [
        {
          id: 10759,
          name: "Action & Adventure",
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
          id: 10762,
          name: "Kids",
        },
        {
          id: 9648,
          name: "Mystery",
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
        {
          id: 37,
          name: "Western",
        },
      ],
    };
  },
  methods: {},
  mounted() {
    axios
      .get(
        "https://api.themoviedb.org/3/tv/" +
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
      this.genres.forEach((el) => {
        if (this.item.genre_ids.includes(el.id)) {
          // console.log(el.name);
          this.mediaGenre = el.name;
        }
      });
      return this.mediaGenre;
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../assets/styles/variables.scss";
#series-card {
  background-color: black;
  height: 400px;
  width: 300px;
  cursor: pointer;
  transition: transform 0.5s;
  &:hover {
    background-color: lighten(black, 10%);
    transform: scale(1.1);
  }
  #card-image {
    height: 100%;
    width: 100%;
    #default-image {
      width: 100%;
      height: 100%;
      object-fit: contain;
      object-position: center;
    }
    img {
      height: 100%;
      width: 100%;
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
}
</style>