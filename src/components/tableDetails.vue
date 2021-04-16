<template>
  <div class="container-fluid">
    <div class="row header">
      <div class="col-4"></div>
      <div class="col-4">
        <h1>Filter table by name</h1>
      </div>
      <div class="col-4"></div>
    </div>
    <div class="row">
      <div class="col-4"></div>
      <div class="col-4">
        <div class="input-group mb-3">
          <div class="input-group-prepend">
            <span class="input-group-text" id="inputGroup-sizing-default">Filter by name</span>
          </div>
          <input type="text" class="form-control" aria-label="Default" aria-describedby="inputGroup-sizing-default"
                 v-model="filter">
        </div>
        <div class="list-group">
          <a v-for="mesa in filterByTable" v-bind:key="mesa.name_table"
             v-bind:tabindex="mesa.id_table" class="btn btn-lg btn-success btn-json" role="button"
             data-toggle="popover" data-trigger="focus" title="Información de la mesa:"
             v-bind:data-content="'ID table: ' + mesa.id_table + ' | ID Zona: '+mesa.id_zone">{{ mesa.name_table }}
          </a>
        </div>
      </div>
      <div class="col-4"></div>
    </div>
  </div>
</template>

<script>
import plano from '../assets/js/plano.json';
import $ from 'jquery'

export default {
  name: "tableDetails",
  data() {
    return {
      plano,
      filter: "",
      tables: [],
    }
  },
  computed: {
    /*
    * Esta funcion devuelve una lista de mesas en funcion del filtro aplicado, si no hay nada escrito en nuestro input
    * nos devolvera la lista entera, pero sin embargo si estamos filtrando, devolvera una array en funcion de si hay o
    * no coincidencias.
    *
    * Esta array es la que maquetamos en el HTML mediante el v-for, asi mostramos listadas las mesas filtradas o no.
    * */
    filterByTable() {
      return this.tables.filter(item => {
        return item.name_table.toLowerCase().includes(this.filter);
      });
    }
  },
  mounted() {
    /*
    * Obtengo del json las mesas con toda su informacion, y las guardo en una array para posteriormente recorrelas
    * */
    let auxTables = [];
    Object.keys(plano.tables).forEach(function (key) {
      auxTables.push(plano.tables[key]);
    });
    this.tables = auxTables;
    // Funcion para añadir el popover
    $(function () {
      $('[data-toggle="popover"]').popover()
    })
  },
}

</script>

<style scoped>

.header {
  margin-top: 1rem;
  margin-bottom: 1rem;
}

.btn-json {
  margin-top: 1rem;
}

.btn-success {
  background-color: #42b983;
  color: #FFFFFF;
  border-color: #42b983;

}

</style>