<template>
<div id="maestro">
<ul v-if = "peliculas">
<p>Seleccione una película para editarla o cree una nueva película<p>
<p><input type="button" name="crear" value="Nueva Pelicula" v-on:click = "showDetalle"/></p>
<li v-for= "pelicula in peliculas" v-on:click= "showDetalle" id="pelicula.Id" v-bind:id = "pelicula.Id">{{pelicula.Titulo}}</li>
</ul>
<div v-else>
<p>No hay películas disponibles cree una nueva película<p>
<p><input type="button" name="crear" value="Nueva Pelicula" v-on:click = "showDetalle"/></p>
</div>
<div id="detalle" v-on:change = "getAll"></div>
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
		EventBus.$on('cambiosUsuario',
			(
				pelicula => {this.getAll();}
				)
			)
		
	},
	methods: {
		getAll: function(){
			axios.get('http://10.60.23.11:50659/api/Peliculas')
			.then (result => {
				this.peliculas = result.data;
			})
			console.log(this.peliculas)
		},
		showDetalle: function(event){
			console.log(event.target.id)
			let pelicula = devuelvePelicula(event.target.id)
			new Vue({
				el: '#detalle',
				data:{
					pelicula:pelicula
				},
				render: h => h(DetallePelicula)
			})
		},
		devuelvePelicula: function(id){
			foreach(var pelicula in this.peliculas){
				if(pelicula.Id == id){
					return pelicula;
				}		
			}
			return {
				Titulo: "",
				Director: "",
				Duracion: null,
				Anno:null,
				Id:null
			} 
		}
	}
	</script>