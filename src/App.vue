<script>
import axios from "axios";

export default {
  data() {
    return {
      baseURL: "http://127.0.0.1:8000/api/characters/",
      player: null,
      cpu: null,
      player_life: 0,
      cpu_life: 0,
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
        this.player_life = response.data.life;
      });
      axios.get(url_cpu).then((response) => {
        this.cpu = response.data;
        this.cpu_life = response.data.life;
      });
    },

    attackPlayer() {
      return this.player.attack * this.randomNumber(2, 6) - this.cpu.defence;
    },
    attackCpu() {
      return this.cpu.attack * this.randomNumber(2, 6) - this.player.defence;
    },

    play() {
      this.cpu_life = this.cpu_life - this.attackPlayer();
      this.player_life = this.player_life - this.attackCpu();
      console.log("Combattimento");

      if (this.cpu_life <= 0) {
        console.log("Player Win");
      } else if (this.player_life <= 0) {
        console.log("cpu Win");
      } else if ((this.cpu_life <= 0) & (this.player_life <= 0)) {
        console.log("Draw");
      }
    },
  },
};
</script>

<template>
  <div
    class="container my-4 d-flex flex-column justify-content-between align-items-center"
  >
    <div class="">
      <button @click="fetchCharacters()" class="btn btn-secondary">
        Generate Character
      </button>
    </div>
    <div class="row mt-5 w-100 align-items-center">
      <div class="col-5">
        <div v-if="player" class="card border-0">
          <h2 class="text-center">Player</h2>
          <div class="img-wrapper d-flex justify-content-center">
            <img :src="player.type.image" alt="" class="img-fluid h-100" />
          </div>
          <div
            class="card-body d-flex flex-column justify-content-center align-items-center"
          >
            <h2>{{ player.name }}</h2>
            <table class="text-center">
              <thead>
                <tr>
                  <th>Attack</th>
                  <th>Defence</th>
                  <th>Life</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td>{{ player.attack }}</td>
                  <td>{{ player.defence }}</td>
                  <td>{{ player_life }}</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
      <div class="col-2">
        <img src="./assets/vs.png" alt="" class="img-fluid" />
      </div>
      <div class="col-5">
        <div v-if="cpu" class="card border-0">
          <h2 class="text-center">CPU</h2>
          <div class="img-wrapper d-flex justify-content-center">
            <img :src="cpu.type.image" alt="" class="img-fluid h-100" />
          </div>

          <div
            class="card-body d-flex flex-column justify-content-center align-items-center"
          >
            <h2>{{ cpu.name }}</h2>
            <table class="text-center">
              <thead>
                <tr>
                  <th>Attack</th>
                  <th>Defence</th>
                  <th>Life</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td>{{ cpu.attack }}</td>
                  <td>{{ cpu.defence }}</td>
                  <td>{{ cpu_life }}</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div class="d-flex justify-content-center align-items-center">
    <button @click="play()" class="btn btn-danger">Fight!</button>
  </div>
</template>

<style lang="scss">
@use "./styles/general.scss";
</style>
