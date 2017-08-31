<template>
  <div id="app" class="row center-block">
    <h3 id="Pelicula" class="col-md-6 btn-secondary btn btn-md" v-on:click="mostrarMaestro"  v-bind:class="{ 'btn-success': peliculaSelected }">Películas</h3>
    <h3 id="Entrada" class="col-md-6 btn-secondary btn btn-md" v-on:click="mostrarMaestro" v-bind:class="{ 'btn-success': entradaSelected }">Entradas</h3>
    <div id="maestro" class="center-block"></div>
  </div>
</template>

<script>
import Vue from 'vue'
import MaestroPeliculas from './MaestroPeliculas.vue'
import MaestroEntradas from './MaestroEntradas.vue'
export default {
  name: 'app',
  data(){
    return {
      peliculaSelected :false,
      entradaSelected:false
    }
    
  },
  methods: {
    mostrarMaestro:function(event){
      let id = event.target.id
      switch (id){
        case "Pelicula":
            this.peliculaSelected=true
            new Vue({
              el: '#maestro',
              render: h => h(MaestroPeliculas)
            })
            this.peliculaSelected?this.entradaSelected=false:null
          break
        case "Entrada":
            this.entradaSelected=true
            new Vue({
              el: '#maestro',
              render: h => h(MaestroEntradas)
            })
            this.entradaSelected?this.peliculaSelected=false:null
          break
      }
    }
  }
}
</script>

