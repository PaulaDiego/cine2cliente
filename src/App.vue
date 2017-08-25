<template>
  <div id="app" class="">
    <h3 id="Pelicula" v-on:click="mostrarPeliculas">Peliculas</h3>
    <div id="maestro"></div>
  </div>
</template>

<script>
import Vue from 'vue'
import MaestroPeliculas from './MaestroPeliculas.vue'
export default {
  name: 'app',
  methods: {
    mostrarPeliculas: function (event) {
      document.getElementById(event.target.id).className = "selected"
      new Vue({
        el: '#maestro',
        render: h => h(MaestroPeliculas)
      })
    }
  }
}
</script>

