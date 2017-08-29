<template>
	<div id="detalle">
		<h3 v-if="entrada.Id">Actualización de Entrada</h3>
		<h3 v-else>Nueva Entrada</h3>
		<input type="hidden" id="id" v-bind:value="entrada.Id"/>
		<p><label for="butaca">Sala: </label><input type="text" id="sala" v-bind:value="entrada.Sala"/></p>
		<p><label for="butaca">Butaca: </label><input type="text" id="butaca" v-bind:value="entrada.Butaca"/></p>
		<p><label for="fila">Fila: </label><input type="number" id="fila" min="1" max="20" v-bind:value="entrada.Fila"/></p>
		<p><label for="precio">Precio: </label><input type="number" id="precio" min="0" v-bind:value="entrada.Precio"/></p>
		<p><label for="dia">Dia: </label><input type="calendar" id="dia" v-bind:value="entrada.Dia"/></p>
		<p><label for="hora">Hora: </label><input type="text" id="hora" v-bind:value="entrada.Hora"/></p>
		<p><label for="pelicula">Película: </label><input type="text" id="pelicula" v-bind:value="entrada.pelicula"/></p>
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
				Entrada:"",
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
				Id:id
			}
			if(id==""){
				entrada.Id = 0

				axios.post('http://10.60.23.11:50659/api/Entradas',entrada)
				.then(
					(entrada)=>{
					alert('Entrada creada con exito')
					this.entrada.Sala = entrada.data.Sala
					this.entrada.Id = entrada.data.Id
					this.entrada.Butaca = entrada.data.Butaca
					this.entrada.Fila = entrada.data.Fila
					this.entrada.Precio = entrada.data.Precio
					this.entrada.Dia = entrada.data.Dia
					this.entrada.Hora = entrada.data.Hora
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