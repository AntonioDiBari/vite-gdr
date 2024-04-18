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
      characters: null,
      result_player: null,


    };
  },

  created() {
    this.fetchCharacters();
  },

  methods: {
    randomNumber(min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min);
    },

    fetchCharacters() {
      axios.get(this.baseURL).then((response) => {
        this.characters = response.data;
      });
    },

    fetchCharactersPlayer(playerId) {
      let url_player = this.baseURL + playerId;



      axios.get(url_player).then((response) => {
        this.player = response.data;
        this.player_life = response.data.life;
      });

    },

    fetchCharactersCpu(cpuId) {

      let url_cpu = this.baseURL + cpuId;




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
        this.result_player = true;
        this.cpu_life = 0;
        setTimeout(() => alert("PLAYER WIN!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!"), 200);
        setTimeout(() => this.player = null, 500);
        setTimeout(() => this.cpu = null, 500);
      } else if (this.player_life <= 0) {
        console.log("cpu Win");
        this.result_player = false;
        this.player_life = 0;
        setTimeout(() => alert("CPU WIN!!!!!!!!!!!!!!!!!!!!!!!!!!"), 200);
        setTimeout(() => this.player = null, 500);
        setTimeout(() => this.cpu = null, 500);
      } else if ((this.cpu_life <= 0) & (this.player_life <= 0)) {
        console.log("Draw");

        this.player_life = 0;
        this.cpu_life = 0;
        setTimeout(() => alert("Draw"), 200);
        setTimeout(() => this.player = null, 500);
        setTimeout(() => this.cpu = null, 500);


      }
    },
  },
};
</script>

<template>
  <div class=" my-5 d-flex flex-column justify-content-between align-items-center main-container">

    <div class=" d-flex justify-content-around main-container">

      <div class="row justify-content-between mt-4 row-main">
        <div class="col-6 col1">

          <h2 class="mb-3 text-center botton-select">Select Player</h2>
          <div class="row justify-content-center">
            <div class="col-1 d-flex justify-content-center p-0 " v-for="character in characters">
              <div :class="{ 'selected': player && player.id === character.id }"
                @click="fetchCharactersPlayer(character.id)" class="card card-char p-1">
                <img :src="character.type.image" alt="" class="img-fluid h-100" />
              </div>

            </div>
          </div>

        </div>

        <div class="col-6 col2">
          <h2 class="mb-3 text-center botton-select">Select Cpu</h2>
          <div class="row justify-content-center">
            <div class="col-1 d-flex justify-content-center p-0 " v-for="character in characters">
              <div :class="{ 'selected': cpu && cpu.id === character.id }" @click="fetchCharactersCpu(character.id)"
                class="card card-char p-1">
                <img :src="character.type.image" alt="" class="img-fluid h-100" />
              </div>

            </div>
          </div>

        </div>
        <!-- <div class="mt-3">
        <button @click="fetchCharactersPlayer()" class="btn btn-primary fs-4">
          Generate Player Character
        </button>
      </div>

      <div class="mt-3">
        <button @click="fetchCharactersCpu()" class="btn btn-primary fs-4">
          Generate Cpu Character
        </button>
      </div>
    </div> -->






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

            <div class="d-flex justify-content-center align-items-center mt-5">
              <button v-if="player" @click="play()" class="btn btn-danger fs-4">
                <i class="fa-solid fa-hand-fist fa-rotate-90"></i> Fight!
                <i class="fa-solid fa-hand-fist fa-rotate-270"></i>
              </button>
            </div>
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


    </div>
  </div>
</template>

<style lang="scss">
@use "./styles/general.scss";
</style>
