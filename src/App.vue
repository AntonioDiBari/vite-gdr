<script>
import axios from "axios";

export default {
  data() {
    return {
      baseURL: "http://127.0.0.1:8000/api/characters/",
      player: [],
      cpu: [],
    };
  },
  methods: {
    randomNumber(min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min);
    },

    fetchCharacters() {
      let url_player = this.baseURL + this.randomNumber(1, 13);
      let url_cpu = this.baseURL + this.randomNumber(1, 13);

      while (url_player === url_cpu) {
        url_cpu = this.baseURL + this.randomNumber(1, 13);
      }

      axios.get(url_player).then((response) => {
        this.player = response.data;
      });
      axios.get(url_cpu).then((response) => {
        this.cpu = response.data;
      });
    },
  },
};
</script>

<template>
  <div class="container my-4">
    <button @click="fetchCharacters()">Gioca</button>
  </div>
</template>

<style lang="scss">
@use "./styles/general.scss";
</style>
