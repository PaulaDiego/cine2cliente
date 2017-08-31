<template>
	<div id="detalle">
		<form onsubmit="return false" v-if="active">
			<h3 v-if="pelicula.Id">Actualización de la película: <span class="text-success">{{pelicula.Titulo}}</span></h3>
			<h3 v-else>Nueva Película</h3>
			<input type="hidden" id="id" v-bind:value="pelicula.Id"/>
			<p><label for="titulo">Título: </label><input type="text" required id="titulo" v-model:value="pelicula.Titulo"/></p>
			<p><label for="director">Director: </label><input type="text" required id="director" v-model:value="pelicula.Director"/></p>
			<p><label for="duracion">Duración: </label><input type="number" required id="duracion" min="0" v-model:value="pelicula.Duracion"/></p>
			<p><label for="anno">Año de lanzamiento: </label><input type="number" required id="anno" min="0" v-model:value="pelicula.Anno"/></p>
			<p>
				<label for="ganero">Género</label>
				<select v-model:value="pelicula.Genero" id="genero" required>
				  <option v-for="option in generos" v-bind:value="option.value">
				    {{ option.text }}
				  </option>
				</select>
			</p>
			<p>
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
export default {
	name: 'detalle',
	data(){
		return {
			pelicula:this.pelicula,
			generos:[
				{text:'Comedia',value:'Comedia'},
				{text:'Drama',value:'Drama'},
				{text:'Terror',value:'Terror'},
				{text:'Otro',value:'otro'}
			],
			active: true
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
				Genero:null,
				Id:null
			}
		}
	},
	methods: {
		eliminar: function(){
			let id = this.$refs.id.value
			axios.delete('http://10.60.23.11:50659/api/Peliculas/'+id)
			 .then(result => {
			 	this.peliculas = result.data;
			 	EventBus.$emit('cambiosPelicula',this.pelicula)
			 	alert('Película eliminada con exito')
			 	this.cerrarDetalle()
			  
			})
			.catch(function(){
				alert("Error al eliminar la película")
			})
			
		},
		aceptar: function(){
			if(this.pelicula.Id==null){
				this.pelicula.Id = 0

				axios.post('http://10.60.23.11:50659/api/Peliculas',this.pelicula)
				.then(
					(pelicula)=>{
						alert('Pelicula creada con exito')
						this.pelicula.Id = pelicula.data.Id
						EventBus.$emit('cambiosPelicula',this.pelicula)
					})
				.catch(function(){
					alert("Error al crear la película")
				})
				
			}else{
				axios.put('http://10.60.23.11:50659/api/Peliculas/'+this.pelicula.Id,this.pelicula)
				.then(
					()=>{
						alert('Película actualizada con exito')
						EventBus.$emit('cambiosPelicula',this.pelicula)
				})
				.catch(function(){
					alert("Error al actualizar la película")
				})
			}
			
		},
		cerrarDetalle:function(){
			this.active = false
		}
	}

}
</script>