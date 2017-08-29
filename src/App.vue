<template>
  <div id="app" class="row center-block">
    <h3 id="Pelicula" class="col-md-6 btn-secondary btn btn-md" v-on:click="mostrarMaestro">Películas</h3>
    <h3 id="Entrada" class="col-md-6 btn-secondary btn btn-md" v-on:click="mostrarMaestro">Entradas</h3>
    <div id="maestro" class="center-block"></div>
    
  </div>
</template>

<script>
import Vue from 'vue'
import MaestroPeliculas from './MaestroPeliculas.vue'
import MaestroEntradas from './MaestroEntradas.vue'
import $ from 'jquery'
export default {
  name: 'app',
  data(){
    return {
      
    }
    
  },
  methods: {
    mostrarMaestro:function(event){
      let id = event.target.id
      switch (id){
        case "Pelicula":
          this.peliculaSelected = true;
          $("#"+id).addClass("btn-success")
          $("#"+id).removeClass("btn-secondary")
          new Vue({
            el: '#maestro',
            render: h => h(MaestroPeliculas)
          })
          $("#Entrada").removeClass("btn-success")
          $("#"+id).addClass("btn-secondary")

          break
        case "Entrada":
          $("#"+id).addClass("btn-success")
          $("#"+id).removeClass("btn-secondary")
          new Vue({
            el: '#maestro',
            render: h => h(MaestroEntradas)
          })
          $("#Pelicula").removeClass("btn-success")
          $("#"+id).addClass("btn-secondary")
          break
      }
    }
  }
}
</script>

