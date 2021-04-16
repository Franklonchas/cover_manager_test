<template>
  <div class="container-fluid">
    <h1 id="title"> Tablemap restaurant</h1>
    <div class="row">
      <div class="container-fluid" v-for="planta in plano.floors" v-bind:key="planta.id">
        <div class="col-12 table" v-bind:id="planta.id_floor">
          <canvas class="canvas" width="920px" height="512px"></canvas>
        </div>
      </div>
    </div>
    <h1>{{ floorName }}</h1>
    <button type="button" @click="previous()" class="btn">Atrás</button>
    <button type="button" @click="next()" class="btn">Siguiente</button>
  </div>
</template>

<script>
import plano from '../assets/js/plano.json'
import table from '../assets/img/table.png'

export default {
  name: "Home",
  data() {
    return {
      plano,
      floors: [],
      nfloors: 0,
      cFloor: 0,
      floorName: 'Planta 0',
    }
  },
  computed: {},
  mounted() {
    /*
    * Obtengo la informacion de las plantas del json y lo guardo en un array
    * Tambien obtengo el numero de plantas que hay, y como cada canvas tiene como id la planta a la que pertenece,
    * solamente muestro cuando carga el componente el primer piso.
    * */
    let keyFloors = Object.keys(plano.floors);
    for (let plantas in keyFloors) {
      this.floors.push(keyFloors[plantas]);
    }
    this.nfloors = this.floors.length - 1;
    document.getElementById(this.floors[0]).style.display = "block";

    /*
    * Aqui mapeo que mesa pertenece a que zona y que zona pertenece a que piso, de manera que asi dibujo en cada
    * correspondiente canvas (planta) las mesas en las coordenadas correspondientes.
    * */
    for (let planta in plano.floors) {
      for (let zona in plano.zones) {
        if (plano.zones[zona].floor === plano.floors[planta].id_floor) {
          for (let mesa in plano.tables) {
            if (plano.zones[zona].id_zone === plano.tables[mesa].id_zone) {
              let currentFloor = document.getElementById(planta).firstChild;
              if (currentFloor.getContext) {
                let ctx = currentFloor.getContext('2d');
                let img = new Image();
                img.src = table;
                img.onload = function () {
                  ctx.drawImage(img, plano.tables[mesa].x, plano.tables[mesa].y);
                }
              }
            }
          }
        }
      }
    }
  },
  methods: {
    /*
    * Funciones para pasar al siguiente o al anterior piso y visualozar asi en bucle los pisos con sus respectivas mesas
    * */
    next: function () {
      if (this.cFloor >= this.nfloors) {
        this.cFloor = 0;
        document.getElementById(this.floors[0]).style.display = "block";
        document.getElementById(this.floors[this.nfloors]).style.display = "none";
        this.floorName = 'Planta ' + this.cFloor
      } else {
        document.getElementById(this.floors[this.cFloor]).style.display = "none";
        this.cFloor++;
        this.floorName = 'Planta ' + this.cFloor
        document.getElementById(this.floors[this.cFloor]).style.display = "block";
      }
    },
    previous: function () {
      if (this.cFloor <= 0) {
        this.cFloor = this.floors.length - 1;
        this.floorName = 'Planta ' + this.cFloor
        document.getElementById(this.floors[this.cFloor]).style.display = "block";
        document.getElementById(this.floors[0]).style.display = "none";
      } else {
        document.getElementById(this.floors[this.cFloor]).style.display = "none";
        this.cFloor--;
        this.floorName = 'Planta ' + this.cFloor
        document.getElementById(this.floors[this.cFloor]).style.display = "block";
      }
    },
  }
}
</script>

<style scoped>
canvas {
  background-color: antiquewhite;
}

button {
  background-color: #42b983;
  color: #FFFFFF;
  padding: 0.5rem;
  margin-left: 1rem;
}

.table {
  display: none;
  margin-top: 1rem;
}

#title {
  margin-top: 1rem;
}
</style>