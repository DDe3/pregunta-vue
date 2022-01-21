<template>
  <h1>Pregunta</h1>
  <!--<img src="https://yesno.wtf/api" alt="">-->
  <img v-show="url!=null" id="img" :src="url" alt="../assets/image.jpg">
  <div class="imagen-opaca"></div>
  <br />
  <div class="contenedor-pregunta">
    <input
      v-model="pregunta"
      type="text"
      placeholder="Ingrese una pregunta"
      @focus="blurFondo"
      @blur="unBlurFondo"
    />
    <p>Recuerde terminar su pregunta con "?"</p>
    <h2>{{ pregunta }}</h2>
  </div>

  <h1>{{ respuesta }}</h1>
</template>

<script>
import { watch } from "@vue/runtime-core";
export default {
  name: "Pregunta",

  data() {
    return {
      pregunta: null,
      respuesta: null,
      url: null,
    };
  },
  methods: {
    blurFondo() {
      document.getElementById("img").style.opacity = 0.8;
    },
    unBlurFondo() {
      document.getElementById("img").style.opacity = 1;
    },

    async obtenerRespuesta() {
      this.respuesta = "Pensando....";
      const data = await fetch("https://yesno.wtf/api").then((ret) =>
        ret.json()
      );
      const { answer, image } = data;
      answer == "yes" ? (this.respuesta = "Si") : (this.respuesta = "No");
      this.url = image;
    },
  },
  watch: {
    pregunta(value, oldValue) {
      if (value.includes("?")) {
        this.unBlurFondo();
        this.obtenerRespuesta();
        // Aqui deberia consumir el API
        console.log("Tiene interrogacion");
      }
    },
  },
};
</script>

<style>
p,
h1,
h2 {
  color: whitesmoke;
  z-index: 99;
  position: relative;
}

p {
  font-size: 10px;
  margin-top: 5px;
}

h2 {
  margin-top: 150px;
}

img,
.imagen-opaca {
  height: 100vh;
  width: 100vw;
  max-height: 100%;
  max-width: 100%;
  position: fixed;
  left: 0px;
  top: 0px;
  z-index: 98;
}
.contenedor-pregunta {
  position: relative;
  z-index: 99;
}

input {
  border-radius: 5px;
  border: none;
  padding: 10px 15px;
  width: 250px;
}

.imagen-opaca {
  background-color: rgba(0, 0, 0, 0.3);
}
</style>