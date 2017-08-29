<template>
	<div id="detalle">
		<h3 v-if="pelicula.Id">Actualización de la película: <span class="text-success">{{pelicula.Titulo}}</span></h3>
		<h3 v-else>Nueva Película</h3>
		<input type="hidden" id="id" v-bind:value="pelicula.Id"/>
		<form v-on:submit>
		<p><label for="titulo">Título: </label><input type="text" required id="titulo" v-bind:value="pelicula.Titulo"/></p>
		<p><label for="director">Director: </label><input type="text" id="director" v-bind:value="pelicula.Director"/></p>
		<p><label for="duracion">Duración: </label><input type="number" id="duracion" min="0" v-bind:value="pelicula.Duracion"/></p>
		<p><label for="anno">Año de lanzamiento: </label><input type="number" id="anno" min="0" v-bind:value="pelicula.Anno"/></p>		<p>
			<input type ="button" name="aceptar" value="Aceptar"  v-on:click="aceptar"/>
			<input v-if="pelicula.Id" type ="button" name="eliminar" value="Eliminar" v-on:click="eliminar"/>
			<input v-else type ="button" name="cancelar" value="Cancelar" v-on:click="cerrarDetalle"/>
		</p>
		</form>
	</div>
</template>

<script>
import {EventBus} from './EventBus.js'
import axios from 'axios'
import $ from 'jquery'
export default {
	name: 'detalle',
	data(){
		return {
			pelicula:this.pelicula
		}
		
	},
	created() {
		if(this.$parent.pelicula != undefined){
			this.pelicula = this.$parent.pelicula
			
		}else{
			this.pelicula = {
				Titulo: "",
				Director: "",
				Duracion: null,
				Anno:null,
				Id:null
			}
		}
	},
	methods: {
		eliminar: function(){
			let id = document.getElementById("id").value
			axios.delete('http://10.60.23.11:50659/api/Peliculas/'+id)
			 .then(result => {
			 	this.peliculas = result.data;
			 	EventBus.$emit('cambiosPelicula',this.pelicula)
			 	alert('Película eliminada con exito')
			 	document.getElementById("detalle").innerHTML = ""
			  
			})
			.catch(function(){
				alert("Error al eliminar la película")
			})
			
		},
		aceptar: function(){
			let id= document.getElementById("id").value
			
			let pelicula = {
				Titulo: document.getElementById("titulo").value,
				Duracion:document.getElementById("duracion").value,
				Director: document.getElementById("director").value,
				Anno:document.getElementById("anno").value,
				Id:id
			}
			if(id==""){
				pelicula.Id = 0

				axios.post('http://10.60.23.11:50659/api/Peliculas',pelicula)
				.then(
					(pelicula)=>{
						alert('Pelicula creada con exito')
						this.pelicula.Id = pelicula.data.Id
						this.pelicula.Titulo = pelicula.data.Titulo
						this.pelicula.Director = pelicula.data.Director
						this.pelicula.Duracion = pelicula.data.Duracion
						this.pelicula.Anno = pelicula.data.Anno
						EventBus.$emit('cambiosPelicula',this.pelicula)
					})
				.catch(function(){
					alert("Error al crear la película")
				})
				
			}else{
				axios.put('http://10.60.23.11:50659/api/Peliculas/'+id,pelicula)
				.then(
					()=>{
						alert('Película actualizada con exito')
						EventBus.$emit('cambiosUsuario',this.pelicula)
				})
				.catch(function(){
					alert("Error al actualizar la película")
				})
			}
			
		},
		cerrarDetalle:function(){

		}
	}

}
</script>