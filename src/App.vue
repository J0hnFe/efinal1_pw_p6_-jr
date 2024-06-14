<template>
  <div class="container" v-if="mostrarJuego">
    <div class="cabecera">
      <h2>Puntaje: {{ puntaje }}</h2>
      <h2>Intento: {{ intento }}</h2>
    </div>

    <Elemento v-bind:nombre="txt1" v-bind:urlImg="url1" />
    <Elemento v-bind:nombre="txt2" v-bind:urlImg="url2" />
    <Elemento v-bind:nombre="txt3" v-bind:urlImg="url3" />

    <button class="jugar" @click="jugar">
      Jugar
    </button>
  </div>


  <div class="ganar" v-if="mostrarGanar">
    <h1>
      Puntaje: {{ puntaje }}
    </h1>
    <h1>
      Felicitaciones has ganado un premio de $10.000.000
    </h1>
    <button class="nuevoJuego" @click="reiniciar">
      Nuevo Juego
    </button>
  </div>

  <div class="perder" v-if="mostrarPerder">
    <h1>
      Has utilizado tus 5 intentos 🤕
    </h1>
    <h1>
      El juego ha terminado
    </h1>
    <button class="nuevoJuego" @click="reiniciar">
      Nuevo Juego
    </button>
  </div>

</template>

<script>
import Elemento from "./components/Elemento.vue";

export default {
  name: "App",
  components: {
    Elemento,
  },

  data() {
    return {
      puntaje: 0,
      intento: 0,
      imgIguales: 0,
      url1: "https://via.placeholder.com/250",
      url2: "https://via.placeholder.com/250",
      url3: "https://via.placeholder.com/250",
      txt1: "xxxxx",
      txt2: "xxxxx",
      txt3: "xxxxx",
      mostrarGanar: false,
      mostrarPerder: false,
      mostrarJuego: true,
    };
  },

  methods: {
    async jugar() {
      const pokemon1 = await this.consumirAPI();
      const pokemon2 = await this.consumirAPI();
      const pokemon3 = await this.consumirAPI();

      this.url1 = pokemon1.url;
      this.txt1 = pokemon1.name;
      this.url2 = pokemon2.url;
      this.txt2 = pokemon2.name;
      this.url3 = pokemon3.url;
      this.txt3 = pokemon3.name;

      this.test();
    },


    async consumirAPI() {
      const pokemonIds = [1, 4, 7, 25, 39]; // Pokemones fijos
      const randomIndex = Math.floor(Math.random() * pokemonIds.length);
      const selectedId = pokemonIds[randomIndex];
      const pokemonData = await fetch(`https://pokeapi.co/api/v2/pokemon/${selectedId}`)
        .then(response => response.json());
      const imageUrl = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${selectedId}.svg`;

      return {
        url: imageUrl,
        name: pokemonData.name
      };
    },

    test() {
      this.intento++;

      if (this.txt1 === this.txt2) this.imgIguales++;
      if (this.txt2 === this.txt3) this.imgIguales++;
      if (this.txt1 === this.txt3) this.imgIguales++;

      if (this.imgIguales === 3) {
        this.puntaje += 5;
      } else if (this.imgIguales === 2) {
        this.puntaje += 2;
      } else {
        this.puntaje += 0;
      }

      if (this.puntaje >= 10) {
        this.mostrarGanar = true;
        this.mostrarPerder = false;
        this.mostrarJuego = false;
      } else if (this.intento >= 5) {
        this.mostrarPerder = true;
        this.mostrarGanar = false;
        this.mostrarJuego = false;
      }
    },

    reiniciar() {
      this.imgIguales = 0,
      this.puntaje = 0,
      this.intento = 0,
      this.url1 = "https://via.placeholder.com/250",
      this.url2 = "https://via.placeholder.com/250",
      this.url3 = "https://via.placeholder.com/250",
      this.txt1 = "xxxxx",
      this.txt2 = "xxxxx",
      this.txt3 = "xxxxx",
      this.mostrarGanar = false,
      this.mostrarPerder = false,
      this.mostrarJuego = true
    },
  },
};
</script>

<style>
img {
  height: 250px;
  width: 250px;
}

body {
  font-size: 15px;
}

.container {
  display: grid;
  font-family: Arial, Helvetica, sans-serif;
  grid-template-columns: repeat(3, 100);
  justify-content: center;
  justify-content: center;
  text-align: center;
  margin: 25px;
}

.cabecera {
  grid-column: span 3;
  display: grid;
  grid-template-columns: repeat(2, 450px);
}

h1,
.jugar {
  grid-column: span 3;
}

.ganar,
.perder {
  font-family: Arial, Helvetica, sans-serif;
  justify-content: center;
  text-align: center;
}

.ganar {
  color: rgb(88, 88, 157);
}

.perder {
  color: red;
}

.nuevoJuego {
  margin: 50px 200px 200px 200px;
  padding: 15px;
  font-size: 25px;
  font-weight: bold;
}

button {
  margin: 50px 200px 200px 200px;
  padding: 15px;
  font-size: 25px;
  font-weight: bold;
}
</style>
