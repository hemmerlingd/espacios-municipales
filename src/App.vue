<template>
  <div id="app">
    <EspaciosMunicipales :lugares="lugares" @cambioCategoria="cambiarCategoria" :cargando="cargando" @cambioEstrellas="buscarEstrellas" :inicial="inicial"></EspaciosMunicipales>
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
    cambiarCategoria: function(valor) {
      if(valor == 0) {
        this.url = this.urlCategoriasBase;
      } else {
        this.url = this.urlBase + '?categorias_id=' + valor;
        this.categoriaSeleccionada = valor;
        console.log(valor);
        var cambioURL = '?categoria=' + valor
        window.history.pushState(window.location.href, 'cambioURL', cambioURL);
      }
      this.getLugares();
    },
    buscarEstrellas: function(valor) {
      if (valor == 0) {
        if (this.categoriaSeleccionada) {
          this.url = this.urlBase + '?categorias_id=' + this.categoriaSeleccionada;
        } else{
          this.url = this.urlCategoriasBase;
        }
      } else {
        if (this.categoriaSeleccionada != 28) {
          this.url = this.urlBase + '?categorias_id=' + this.categoriaSeleccionada;
        }else{
          this.url = this.urlBase + '?estrellas=' + valor + '&categorias_id=' + this.categoriaSeleccionada;
        }
      }
      this.getLugares();
    },
    getLugares() {
      let v = this;
      this.cargando = true;
      axios.get(this.url)
      .then(function(response) {
        v.lugares = response.data.results;
        v.cargando =false;
      })
      .catch(function() {
        v.cargando = false;
      })
    },
  },
  mounted: function() {
    this.getLugares();
  },
  data: function() {
    return{
      lugares: [],
      categoriaSeleccionada: 13,
      url: "https://gobiernoabierto.cordoba.gob.ar/api/lugar-actividad/?categorias_id=13",
      urlBase: "https://gobiernoabierto.cordoba.gob.ar/api/lugar-actividad/",
      urlCategoriasBase: "https://gobiernoabierto.cordoba.gob.ar/api/lugar-actividad/?categorias_id=13",
      cargando: false,
      inicial: 13
    }
  },
   created()
  {
    let uri = window.location.href.split('?');
    if (uri.length == 2)
    {
      let vars = uri[1].split('&');
      let parametro = '';
      let tmp = '';
      vars.forEach(function(v){
        tmp = v.split('=');
        if(tmp.length == 2)
        parametro = tmp[1];
      });
      console.log(parametro);
      this.cambiarCategoria(parametro);
      this.inicial = parametro;
    }
  },
  updated(){
  },
}

</script>

<style>
#app {
  text-align: center;
}
.pagination {
  cursor: pointer;
}
.pagination>.active>a, .pagination>.active>a:focus, .pagination>.active>a:hover, .pagination>.active>span, .pagination>.active>span:focus, .pagination>.active>span:hover {
  background-color: #00a665;
  border-color: #008f57;
}
.pagination>li>a:focus, .pagination>li>a:hover, .pagination>li>span:focus, .pagination>li>span:hover,.pagination>li>a, .pagination>li>span {
  color: #00a665;
}
</style>
