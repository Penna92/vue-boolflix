<template>
  <div
    @mouseover="hover = false"
    @mouseleave="hover = true"
    id="series-card"
    :class="{ container: !hover }"
    class="card d-flex flex-column justify-content-between text-center"
  >
    <div v-if="hover === true" id="card-image">
      <img
        v-if="item.poster_path === null"
        id="poster"
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
      <p>
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
    </div>
  </div>
</template>

<script>
export default {
  name: "AppSeriesCard",
  props: ["item"],
  data() {
    return {
      imgUrl: "https://image.tmdb.org/t/p/w342",
      hover: true,
    };
  },
};
</script>

<style lang="scss" scoped>
@import "../assets/styles/variables.scss";
#series-card {
  background-color: black;
  height: 400px;
  cursor: pointer;
  transition: transform 0.5s;
  &:hover {
    background-color: lighten(black, 10%);
    transform: scale(1.1);
  }
  #card-image {
    height: 100%;
    width: 100%;
    img {
      height: 100%;
      width: 100%;
      object-fit: contain;
    }
  }
  div {
    overflow: hidden;
    p {
      font-size: 18px;
      color: white;
      font-weight: bold;
      span {
        font-size: 16px;
        font-weight: 300;
      }
    }
    span {
      #flag {
        margin-left: 5px;
        width: 30px;
        height: 20px;
      }
    }
  }
}
</style>