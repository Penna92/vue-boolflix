<template>
  <div
    id="series-card"
    class="
      container
      card
      d-flex
      flex-column
      justify-content-between
      text-center
    "
  >
    <div id="card-image">
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
    <div class="d-flex flex-column align-items-center">
      <h6 class="my-3">{{ item.name }}</h6>
      <p>{{ item.original_name }}</p>
      <p
        v-if="
          item.original_language !== 'it' &&
          item.original_language !== 'en' &&
          item.original_language !== 'fr' &&
          item.original_language !== 'es' &&
          item.original_language !== 'de'
        "
      >
        {{ item.original_language }}
      </p>
      <img
        id="flag"
        class="mb-3"
        v-if="
          item.original_language === 'it' ||
          item.original_language === 'en' ||
          item.original_language === 'fr' ||
          item.original_language === 'es' ||
          item.original_language === 'de'
        "
        :src="require('../assets/img/' + item.original_language + '.png')"
        :alt="item.original_language"
      />
      <p>Voto: {{ item.vote_average }}</p>
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
    };
  },
};
</script>

<style lang="scss" scoped>
@import "../assets/styles/variables.scss";
#series-card {
  background-color: rgba(0, 0, 0, 0.4);
  height: 400px;
  cursor: pointer;
  transition: transform 0.5s;
  &:hover {
    background-color: lighten(black, 10%);
    transform: scale(1.1);
  }
  #card-image {
    height: 40%;
    img {
      margin-top: 20px;
      height: 90%;
      width: 80%;
    }
  }
  div {
    width: 100%;
    height: 60%;
    h6 {
      text-transform: uppercase;
      color: white;
    }
    p {
      color: white;
    }
    #flag {
      width: 30px;
      height: 20px;
    }
  }
}
</style>