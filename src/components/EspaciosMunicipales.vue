<template>
	<div class="contenedor">
		<div class="row">
			<div class="col-md-4 col-md-offset-4 filtros">
				<label class="caption">Categorías:</label>
				<select class="form-control filtro__categoria" v-model="categoria">
					<option selected value="">Todos</option>
					<option value="16">Centro Comercial</option>
					<option value="15">Teatro</option>
					<option value="14">Gastronomia</option>
					<option value="13">Alojamiento</option>
					<option value="12">Centralidad</option>
					<option value="11">Oficina de Turismo</option>
					<option value="10">Centro Deportivo</option>			
					<option value="9">Paseo</option>
					<option value="8">Peatonal</option>
					<option value="7">Plaza</option>
					<option value="6">Iglesia</option>
					<option value="5">Escuela/Universidad</option>
					<option value="4">Museo</option>
					<option value="3">Centro Vecinal</option>
					<option value="2">CPC</option>
					<option value="1">Espacio Cultural</option>
				</select>
				<label class="caption">Nombre:</label>
				<input type="text" class="form-control" v-model="nombre" placeholder="Ingrese el Nombre">
			</div>

		</div>
		<div class="row">
			<paginate name="lugares" :list="lugares" :per="12" v-if="lugares.length>0" class="col-md-12 resultados">
				<div v-show="!cargando" v-for="lugar in paginated('lugares')" :key="lugar.id" class="panel panel-default lugar">
					<a :href="'http://www.cordoba.gob.ar/sitiosdeinteres/' + lugar.id + '/' + nombreToUrl(lugar.nombre)" target="_blank" >
					<div class="lugar__foto panel-body" ><img v-if="lugar.fotos[0]" class="foto__img" :src="lugar.fotos[0].foto.original"/><img v-else class="foto__img" src="https://www.cordoba.gob.ar/wp-content/uploads/2017/10/catedral_de_crdoba_a_la_tarde.jpg"/></div>
					<span class="lugar__nombre panel-footer">{{lugar.nombre}}</span>
				</a></div>
			</paginate>
			<div class="resultados__null" v-else>Sin Resultados</div>

			<nav class="Page navigation">
		  		<paginate-links class="pagination" v-if="lugares.length>12" for="lugares" :limit="3"  :show-step-links="true" :step-links="{
			    	next: 'Proximo >>',
			    	prev: '<< Anterior'
				  }" >
				</paginate-links>
			</nav>
		</div>
	</div>
</template>

<script>

export default {

  name: 'EspaciosMunicipales',
  watch: {
    categoria: function(valor){
        this.$emit('cambioCategoria',valor)
      },
     nombre:  function(valor){
        this.$emit('cambioNombre',valor)
      } 
    },
  data: function(){
      return{
      categoria: null,
      paginate:['lugares'],
      nombre: null
      }
  },
  methods: {
  	nombreToUrl(nombre) {
        return nombre.toLowerCase().replace(/[^a-z0-9]/g, '-').replace(/\-+/g, '-');
      }
  },
  props: ["lugares", "cargando"]

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
		border-radius: 10px;
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
		border-radius: 10px 10px 0 0;
	}
	.lugar__nombre {
		font-weight: 600;
		display: block;
		min-height: 75px;
		padding-top: 20px;
		border-radius: 0 0 10px 10px;
	}
	.resultados__null {
		font-weight: 600;
	}
	.row {
		margin-bottom: 25px;
	}
	.lugar:hover {
  animation: bounce-in .5s;
	}
	.lugar:hover {
	  animation: bounce-in .5s reverse;
	}
	@keyframes bounce-in {
	  50% {
	    transform: scale(1.1);
	  }
	}
	.pagination {
		cursor: pointer;
	}

</style>