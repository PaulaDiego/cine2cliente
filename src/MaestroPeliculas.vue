<template>
<div id="maestro">
	<ul v-if = "peliculas" class="list-group">
	<p>Seleccione una película para editarla o cree una nueva película</p>
	<p><input type="button" name="crear" value="Nueva Pelicula" v-on:click = "showDetalle"/></p>
	<a href="#" class="list-group-item row col-md-4" v-for= "pelicula in peliculas" v-on:click= "showDetalle" id="pelicula.Id" v-bind:id = "pelicula.Id">{{pelicula.Titulo}}</a>
	</ul>
	<div v-else>
	<p>No hay películas disponibles cree una nueva película</p>
	<p><input type="button" name="crear" value="Nueva Película" v-on:click = "showDetalle"/></p>
	</div>
	<div id="detalle"></div>
	</div>
</template>

<script>
import axios from 'axios'
import Vue from 'vue'
import DetallePelicula from './DetallePelicula.vue'
import {EventBus} from './EventBus.js'
export default{
	name: 'maestro',
	data () {
		return {
			peliculas: null
		}
	},
	created(){
		this.getAll()
		EventBus.$on('cambiosPelicula',
			
				this.getAll
				
			)
		
	},
	methods: {
		getAll: function(){
			axios.get('http://10.60.23.11:50659/api/Peliculas')
			.then (result => {
				this.peliculas = result.data;
			})
		},
		showDetalle: function(event){
			let pelicula = this.devuelvePelicula(event.target.id)
			new Vue({
				el: '#detalle',
				data:{
					pelicula:pelicula
				},
				render: h => h(DetallePelicula)
			})
		},
		devuelvePelicula: function(id){
			var resultado
			this.peliculas.forEach(function(pelicula){
				if(pelicula.Id == id){
					resultado = pelicula
				}		
			
			})
			return resultado
		}
	}
}
</script>