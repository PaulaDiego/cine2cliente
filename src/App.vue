<template>
  <div id="app" class="">
    <h3 id="Pelicula" v-on:click="mostrarMaestro">Peliculas</h3>
    <h3 id="Entrada" v-on:click="mostrarMaestro">Entradas</h3>
    <div id="maestro"></div>
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
      peliculaSelected:false,
      entradaSelected : false
    }
    
  },
  methods: {
    mostrarMaestro:function(event){
      let id = event.target.id
      switch (id){
        case "Pelicula":
          if(this.peliculaSelected){
            this.peliculaSelected = false
            $("#maestroPeliculas").remove()
            $("#"+id).removeClass("selected")
          }else{
            this.peliculaSelected = true;
            $("#"+id).addClass("selected")
          }
          $("#Entrada").removeClass("selected")
          new Vue({
            el: '#maestro',
            render: h => h(MaestroPeliculas)
          })

          break
        case "Entrada":
        if(this.entradaSelected){
            this.entradaSelected = false
            $("#maestroEntradas").remove()
            $("#"+id).removeClass("selected")

          }else{
            this.entradaSelected = true;
            $("#"+id).addClass("selected")
          }
          $("#Pelicula").removeClass("selected")
          new Vue({
            el: '#maestro',
            render: h => h(MaestroEntradas)
          })
          break
      }
    }
  }
}
</script>

