<template>
  <div id="app">
    <header>
      <h1>Mi Música</h1>
    </header>
    <main>
      <section class="reproductor">
        <h2 class="nombre-cancion">
          {{ actual.nombre }} -
          <span>{{ actual.artista }}</span>
        </h2>
        <div class="control">
          <button class="anterior" @click="anterior">Anterior</button>
          <button class="play" v-if="!reproduciendo" @click="play">Play</button>
          <button class="pausa" v-else @click="pausa">Pausa</button>
          <button class="siguiente" @click="siguiente">Siguiente</button>
        </div>
      </section>
      <section class="playlist">
        <h3>Lista de Reproduccion</h3>
        <button
          v-for="cancion in canciones"
          :key="cancion.src"
          @click="play(cancion)"
          :class="
            cancion.src == actual.src ? 'cancion reproduciendo' : 'cancion'
          "
        >
          {{ cancion.nombre }} - {{ cancion.artista }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      actual: {},
      index: 0,
      reproduciendo: false,
      canciones: [
        {
          nombre: "Claro de Luna",
          artista: "Piperno",
          src: require("./assets/clarodeluna.mp3"),
        },
        {
          nombre: "Danza Gris",
          artista: "Piperno",
          src: require("./assets/danzagris.mp3"),
        },
        {
          nombre: "Red Eterea",
          artista: "Piperno",
          src: require("./assets/redeterea.mp3"),
        },
        {
          nombre: "Atrapados",
          artista: "Piperno",
          src: require("./assets/atrapados.mp3"),
        },
        {
          nombre: "Silencio Extraño",
          artista: "Piperno",
          src: require("./assets/silencioextraño.mp3"),
        },
      ],
      reproductor: new Audio(),
    };
  },
  methods: {
    play(cancion) {
      if (typeof cancion.src != "undefined") {
        this.actual = cancion;
        this.reproductor.src = this.actual.src;
      }
      this.reproductor.play();
      this.reproductor.addEventListener(
        "ended",
        function() {
          this.index++;

          if (this.index > this.canciones.length - 1) {
            this.index = 0;
          }
          this.actual = this.canciones[this.index];
          this.play(this.actual);
        }.bind(this)
      );
      this.reproduciendo = true;
    },
    pausa() {
      this.reproductor.pause();
      this.reproduciendo = false;
    },
    siguiente() {
      this.index++;
      if (this.index > this.canciones.length - 1) {
        this.index = 0;
      }
      this.actual = this.canciones[this.index];
      this.play(this.actual);
    },
    anterior() {
      this.index--;
      if (this.index < 0) {
        this.index = this.canciones.length - 1;
      }
      this.actual = this.canciones[this.index];
      this.play(this.actual);
    },
  },
  created() {
    this.actual = this.canciones[this.index];
    this.reproductor.src = this.actual.src;
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
  background-color: #e8e8e8;
}

header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: #5588a3;
  color: #fff;
}

main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}

.nombre-cancion {
  color: #212121;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}

.nombre-cancion span {
  font-weight: 400;
  font-style: italic;
}

.control {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px 15px;
}

button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}

button:hover {
  opacity: 0.8;
}

.play,
.pausa {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: #fff;
  background-color: #a9c6de;
}

.anterior,
.siguiente {
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color: #fff;
  background-color: #5ca0d3;
}

.playlist {
  padding: 0px 30px;
}

.playlist h3 {
  color: #212121;
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center;
}

.playlist .cancion {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}

.playlist .song:hover {
  color: #5ca0d3;
}

.playlist .cancion.reproduciendo {
  color: #fff;
  background-image: linear-gradient(to right, #c8e6f5, #5ca0d3);
}
</style>
