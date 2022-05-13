<template>
  <div>
    <select
      class="form-select mt-3"
      name="genre"
      id="genre"
      v-model="selectValue"
      @change="searchGenre"
    >
      <option value="">Filtra per genere</option>
      <option :value="genre" v-for="genre in mediaGenres" :key="genre.id">
        {{ genre.name }}
      </option>
    </select>
  </div>
</template>

<script>
export default {
  name: "AppSelectGenre",
  data() {
    return {
      selectValue: "",
    };
  },
  props: ["mediaGenres"],
  methods: {
    searchGenre() {
      this.$emit("searchGenre", this.selectValue);
      // this.selectValue = "";
    },
    // reset() {
    //   this.selectValue = "";
    //   this.$emit("search", this.selectValue);
    // },
  },
};
</script>

<style lang="scss">
select {
  width: 200px !important;
}
</style>