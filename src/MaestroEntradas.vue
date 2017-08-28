<template>
<div id="maestro">
	<div id="maestroEntradas">
		<ul v-if = "entradas">
		<p>Seleccione una entrada para editarla o cree una nueva entrada<p>
		<p><input type="button" name="crear" value="Nueva Entrada" v-on:click = "showDetalle"/></p>
		<li v-for= "entrada in entradas" v-on:click= "showDetalle" id="entrada.Id" v-bind:id = "entrada.Id">{{entrada.Fila}}</li>
		</ul>
		<div v-else>
		<p>No hay entradas disponibles cree una nueva entrada<p>
		<p><input type="button" name="crear" value="Nueva Entrada" v-on:click = "showDetalle"/></p>
		</div>
		<div id="detalle" v-on:change = "getAll"></div>
	</div>
</div>
</template>

<script>
import axios from 'axios'
import Vue from 'vue'
import DetalleEntrada from './DetalleEntrada.vue'
import {EventBus} from './EventBus.js'
import $ from 'jquery'
export default{
	name: 'maestro',
	data () {
		return {
			entradas: null
		}
	},
	created(){
		this.getAll()
		EventBus.$on('cambiosEntrada',
			this.getAll
			)
		
	},
	methods: {
		getAll: function(){
			axios.get('http://10.60.23.11:50659/api/Entradas')
			.then (result => {
				this.entradas = result.data;
			})
		},
		showDetalle: function(event){
			console.log(event.target.id)
			let entrada = this.devuelveEntrada(event.target.id)
			new Vue({
				el: '#detalle',
				data:{
					entrada:entrada
				},
				render: h => h(DetalleEntrada)
			})
		},
		devuelveEntrada: function(id){
			var resultado
			this.entradas.forEach(function(entrada){
				if(entrada.Id == id){
					resultado = entrada
				}		

			})
			return resultado
		}
	}
}
</script>