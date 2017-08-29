<template>
	<div id="detalle">
		<h3 v-if="entrada.Id">Actualización de Entrada</h3>
		<h3 v-else>Nueva Entrada</h3>
		<form v-on:submit.prevent>
		<input type="hidden" id="id" v-model:value="entrada.Id"/>
		<p><label for="butaca">Sala: </label><input type="text" required id="sala" v-model:value="entrada.Sala"/></p>
		<p><label for="butaca">Butaca: </label><input type="text" required id="butaca" v-model:value="entrada.Butaca"/></p>
		<p><label for="fila">Fila: </label><input type="number" required id="fila" min="1" max="20" v-model:value="entrada.Fila"/></p>
		<p><label for="precio">Precio: </label><input type="number" required id="precio" min="0" v-model:value="entrada.Precio"/></p>
		<p><label for="dia">Dia: </label><input type="number" required id="dia" v-model:value="entrada.Dia"/></p>
		<p><label for="hora">Hora: </label><input type="text" required id="hora" v-model:value="entrada.Hora"/></p>
		<p><label for="pelicula">Película: </label><input type="text" required id="pelicula" v-model:value="entrada.Pelicula"/></p>
		</form>
		<p>
			<input type ="button" name="aceptar" value="Aceptar"  v-on:click="aceptar"/>
			<input v-if="entrada.Id" type ="button" name="eliminar" value="Eliminar" v-on:click="eliminar"/>
			<input v-else type ="button" name="cancelar" value="Cancelar" v-on:click="cerrarDetalle"/>
		</p>
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
			entrada:this.entrada
		}
		
	},
	created() {
		if(this.$parent.entrada != undefined){
			this.entrada = this.$parent.entrada
			
		}else{
			this.entrada = {
				Sala: "",
				Butaca: "",
				Fila: "",
				Precio: null,
				Dia:"",
				Hora:"",
				Pelicula:"",
				Id: null
			}
		}
	},
	methods: {
		eliminar: function(){
			let id = document.getElementById("id").value
			axios.delete('http://10.60.23.11:50659/api/Entradas/'+id)
			 .then(result => {
			 	this.entradas = result.data
			 	EventBus.$emit('cambiosEntrada',this.entrada)
			 	alert('Entrada eliminada con exito')
			 	document.getElementById("detalle").innerHTML = ""
			 	$("#form").remove()
			})
			.catch(function(){
				alert("Error al eliminar la entrada")
			})
			
		},
		aceptar: function(){
			let id= document.getElementById("id").value
			
			let entrada = {
				Sala: document.getElementById("sala").value,
				Butaca: document.getElementById("butaca").value,
				Fila:document.getElementById("fila").value,
				Precio: document.getElementById("precio").value,
				Dia:document.getElementById("dia").value,
				Hora:document.getElementById("hora").value,
				Pelicula:document.getElementById("pelicula").value,
				Id:id
			}
			if(id==""){
				entrada.Id = 0

				axios.post('http://10.60.23.11:50659/api/Entradas',entrada)
				.then(
					(entrada)=>{
					alert('Entrada creada con exito')
					this.entrada.Id = entrada.data.Id
					EventBus.$emit('cambiosEntrada',this.entrada)
				})
				.catch(function(){
					alert("Error al crear la entrada")
				})
			}else{
				axios.put('http://10.60.23.11:50659/api/Entradas/'+id,entrada)
				.then(
					()=>{
						alert('Entrada actualizada con exito')
						EventBus.$emit('cambiosEntrada',this.entrada)
				})
				.catch(function(){
					alert("Error al actualizar la entrada")
				})
			}
			
		},
		cerrarDetalle:function(){

		}
	}

}
</script>