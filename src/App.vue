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

    fetchCharactersPlayer() {
      let url_player = this.baseURL + this.randomNumber(1, 6);


      // while (url_player === url_cpu) {
      //   url_cpu = this.baseURL + this.randomNumber(1, 13);
      // }

      axios.get(url_player).then((response) => {
        this.player = response.data;
        this.player_life = response.data.life;
      });

    },

    fetchCharactersCpu() {

      let url_cpu = this.baseURL + this.randomNumber(7, 13);

      // while (url_cpu === url_player) {
      //   url_cpu = this.baseURL + this.randomNumber(1, 13);
      // }


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
        this.cpu_life = 0;
        setTimeout(() => alert("Player Win!!!"), 500);
      } else if (this.player_life <= 0) {
        console.log("cpu Win");
        this.player_life = 0;
        setTimeout(() => alert("Cpu Win!!!"), 500);
      } else if ((this.cpu_life <= 0) & (this.player_life <= 0)) {
        console.log("Draw");
        this.player_life = 0;
        this.cpu_life = 0;
        setTimeout(() => alert("Draw"), 500);
      }
    },
  },
};
</script>

<template>
  <div class="container my-5 d-flex flex-column justify-content-between align-items-center">

    <div class="container d-flex justify-content-around">

      <div class="mt-3">
        <button @click="fetchCharactersPlayer()" class="btn btn-primary fs-4">
          Generate Player Character
        </button>
      </div>

      <div class="mt-3">
        <button @click="fetchCharactersCpu()" class="btn btn-primary fs-4">
          Generate Cpu Character
        </button>
      </div>
    </div>

    <div class="row mt-5 w-100 align-items-center">
      <div class="col-5">
        <div v-if="player" class="card border-0">
          <h2 class="text-center">Player</h2>
          <div class="img-wrapper d-flex justify-content-center">
            <img :src="player.type.image" alt="" class="img-fluid h-100" />
          </div>
          <div class="card-body d-flex flex-column justify-content-center align-items-center">
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
                  <td class="text-danger">
                    <strong>{{ player_life }}</strong>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
      <div v-if="player, cpu" class="col-2">
        <img src="./assets/vs.png" alt="" class="img-fluid" />
      </div>
      <div class="col-5">
        <div v-if="cpu" class="card border-0">
          <h2 class="text-center">CPU</h2>
          <div class="img-wrapper d-flex justify-content-center">
            <img :src="cpu.type.image" alt="" class="img-fluid h-100" />
          </div>

          <div class="card-body d-flex flex-column justify-content-center align-items-center">
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
                  <td class="text-danger">
                    <strong>{{ cpu_life }}</strong>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div class="d-flex justify-content-center align-items-center">
    <button v-if="player" @click="play()" class="btn btn-danger fs-4">
      <i class="fa-solid fa-hand-fist fa-rotate-90"></i> Fight!
      <i class="fa-solid fa-hand-fist fa-rotate-270"></i>
    </button>
  </div>
</template>

<style lang="scss">
@use "./styles/general.scss";
</style>
