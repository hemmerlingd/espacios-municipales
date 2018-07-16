<template>
	<div class="contenedor">
		<div class="row">
			<h1>ALOJAMIENTOS</h1>
			<div class="form-inline filtros">
				<div class="form-group">
					<label class="form-control-static">Tipo:
					<select class="form-control filtro__categoria" v-model="categoria">
						<option selected value="13">Todos</option>
						<option value="26">Apart Hotel</option>
						<option value="27">Camping</option>
						<option value="25">Hostel</option>
						<option value="28">Hotel</option>			
						<option value="29">Hotel Boutique</option>	
						<option value="30">Otros</option>			
					</select></label>
				</div>
				<div class="form-group" v-if="categoria==28">
					<label class="form-control-static"  >Estrellas:</label>
					<select class="form-control filtro__estrellas" v-model="estrellas" >
						<option selected value="0">Todos</option>
						<option value="1">⭐</option>
						<option value="2">⭐⭐</option>
						<option value="3">⭐⭐⭐</option>			
						<option value="4">⭐⭐⭐⭐</option>			
						<option value="5">⭐⭐⭐⭐⭐</option>
					</select>
				</div>
			</div>
		</div>
		<div class="row">
			<paginate name="lugares" :list="lugares" :per="12" v-if="lugares.length>0" class="col-md-12 resultados">
				<div v-show="!cargando" v-for="lugar in paginated('lugares')" :key="lugar.id" class="panel panel-default lugar">
					<a :href="'/que-hacer/sitiosdeinteres/' + lugar.id + '/' + nombreToUrl(lugar.nombre)" >
						<div class="lugar__foto panel-body" ><img v-if="lugar.fotos[0]" class="foto__img" :src="lugar.fotos[0].foto.original"/><img v-else class="foto__img" src="https://www.cordoba.gob.ar/wp-content/uploads/2017/10/catedral_de_crdoba_a_la_tarde.jpg"/></div>
						<div class="panel-footer"><span class="lugar__nombre"><h4>{{lugar.nombre}}</h4><span class="lugar__direccion"><span class="fa fa-map-marker"></span>{{lugar.direccion}}</span></span></div>
					</a></div>
				</paginate>
				<div class="resultados__null" v-else>Sin Resultados</div>

				<paginate-links class="pagination" v-if="lugares.length>12" for="lugares" :limit="3"  :show-step-links="true" :step-links="{
						next: 'Siguiente',
						prev: 'Anterior'
					}" >
				</paginate-links>
		</div>
	</div>
</template>

<script>

export default {

	name: 'EspaciosMunicipales',
	watch: {
		categoria: function(valor){
			this.$emit('cambioCategoria',valor);
			this.estrellas="0";
		},
		estrellas:  function(valor){
			this.$emit('cambioEstrellas',valor)
		}
	},
	data: function(){
		return{
			categoria: 13,
			paginate: ['lugares'],
			estrellas: null		
		}
	},
	methods: {
		nombreToUrl(nombre) {
			return nombre.toLowerCase().replace(/[^a-z0-9]/g, '-').replace(/\-+/g, '-');
		}
	},
	created(){
		if (this.inicial){
			this.categoria = this.inicial;
		}
	},
	props: ["lugares", "cargando", "inicial"]
}
</script>
<style scoped>
.resultados {
	display: flex;
	flex-wrap: wrap;
	justify-content: center;
}

.lugar {
	width: 350px;
	min-height: 275px;
	margin: 5px;
	cursor: pointer;
}
.lugar__foto {
	height: 200px;
	width: 100%;
	padding: 0px;
}
.foto__img {
	display: block;
	object-fit: cover;
	width: 100%;
	height: 100%;
}
.lugar__nombre {
	font-weight: 600;
	display: block;
	min-height: 75px;
}
.resultados a:hover {
	text-decoration: none;
}
.resultados__null {
	font-weight: 600;
}
.row {
	margin-bottom: 25px;
}
</style>