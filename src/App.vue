<template>
  <div id="app">
    <EspaciosMunicipales :lugares="lugares" @cambioCategoria="cambiarCategoria" :cargando="cargando" @cambioNombre="buscarNombre">
      
    </EspaciosMunicipales>

  </div>
</template>

<script>
import EspaciosMunicipales from './components/EspaciosMunicipales';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    EspaciosMunicipales
  },
  methods: {
    cambiarCategoria: function(valor){
      if(valor == 0){
        this.url = this.urlBase;
      }
      else {
        this.url = this.urlBase + '?categorias_id=' + valor;
        this.categoriaSeleccionada = valor;
    }
      this.getLugares();
    },
        buscarNombre: function(valor){
      if(valor == 0){
        if (this.categoriaSeleccionada){
          this.url = this.urlBase + '?categorias_id=' + valor;
        }else{
        this.url = this.urlBase;
        }
      }
      else {
        if (this.categoriaSeleccionada == null){
        this.url = this.urlBase + '?q=' + valor;
      }else{
        this.url = this.urlBase + '?q=' + valor + '&categorias_id=' + this.categoriaSeleccionada;
      }
    }
      this.getLugares();
    },
    getLugares() {
      let v = this;
      this.cargando = true;
        axios.get(this.url)
      .then(function(response){
        v.lugares = response.data.results;
        v.cargando =false;
      })
      .catch(function(){
        v.cargando = false;
      })
    }
  },
  mounted: function(){
    this.getLugares();
  },
  data: function(){
      return{
      categoria: null,
      lugares: [],
      categoriaSeleccionada: null,
      url: "https://gobiernoabierto.cordoba.gob.ar/api/lugar-actividad/",
      urlBase: "https://gobiernoabierto.cordoba.gob.ar/api/lugar-actividad/",
      cargando: false
      }
  }
}

</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
