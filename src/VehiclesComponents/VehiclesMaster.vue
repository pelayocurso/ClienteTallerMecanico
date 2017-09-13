<template>
<div class="master row">
  <alert></alert>

  <div class="list" :class="{'col-lg-12' : !show_detail, 'col-lg-6': show_detail}">
    <button class="btn btn-primary btn-crear" @click="handleCreateNewVehicleClick">Crear nuevo vehiculo</button>

    <div class="table-responsive">
      <table class="table table-hover">
        <thead>
          <tr>
            <th>Identificador</th>
            <th>Dueño</th>
            <th>Matricula</th>
            <th>Fecha(Mes/Dia/Año)</th>
            <th>Acciones</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="vehicle in vehicles">
            <td> {{ vehicle.Id }} </td>
            <td> {{ vehicle.Owner }} </td>
            <td> {{ vehicle.Registration }} </td>
            <td> {{ formatDate(vehicle.Date) }} </td>
            <td class="master-button-group">
              <button class="btn btn-default" @click="handleEditClick(vehicle)">Editar</button>
              <button class="btn btn-danger" @click="handleDeleteClick(vehicle)">Borrar</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>

  <vehicle @addVehicle="onAddVehicle" @modifyVehicle="onModifyVehicle" @closeForm="onCloseForm" :api_host="host"></vehicle>
</div>
</template>

<script>
import Vehicle from './Vehicle.vue';
import Alert from '../SingleComponents/Alert.vue';
import axios from 'axios';

export default {
  name: 'vehicle-master',

  components: {
    Vehicle,
    Alert
  },

  data() {
    return {
      vehicles: [],
      host: 'http://localhost:50072/api/Vehicles',
      show_detail: false,
    };
  },

  created() {
    this.getAllVehicles();
  },

  methods: {
    /* SERVER REQUESTS */
    getAllVehicles() {
      axios.get(this.host)
        .then((response) => {
          this.vehicles = response.data;
          Vue.$emit('show-alert', 'success', "<span class='glyphicons glyphicons-ok'></span> Vehiculos cargados satisfactoriamente.");
        }).catch((error) => {
          Vue.$emit('show-modal', error.message, error.stack);
        });
    },

    deleteVehicle(vehicle) {
      axios.delete(this.host + '/' + vehicle.Id)
        .then((response) => {
          let index = this.vehicles.indexOf(vehicle);
          this.vehicles.splice(index, 1);
          Vue.$emit('show-alert', 'success', "<span class='glyphicons glyphicons-ok'></span> Vehiculo borrado satisfacoriamente.");
        }).catch((error) => {
          Vue.$emit('show-modal', error.message, error.stack);
        });
    },

    /* HANDLE SELF EVENTS */
    handleCreateNewVehicleClick() {
      this.show_detail = true;
      Vue.$emit('show-form', null);
    },

    handleEditClick(vehicle) {
      this.show_detail = true;
      Vue.$emit('show-form', vehicle);
    },

    handleDeleteClick(vehicle) {
      Vue.$emit('show-modal', '¿Seguro?', "Esta a punto de borrar un vehiculo", this.deleteVehicle, vehicle);
      //this.deleteVehicle(vehicle);
    },

    /* HANDLE CHILDREN EVENTS */
    onAddVehicle(vehicle) {
      Vue.$emit('show-alert', 'success', "<span class='glyphicons glyphicons-ok'></span> Vehiculo creado satisfacoriamente.");
      this.vehicles.push(vehicle);
      this.show_detail = false;
    },

    onModifyVehicle(vehicleModified) {
      Vue.$emit('show-alert', 'success', "<span class='glyphicons glyphicons-ok'></span> Vehiculo modificado satisfacoriamente.");
      let index = null;
      this.vehicles.forEach((vehicle) => {
        if (vehicle.Id == vehicleModified.Id) {
          index = this.vehicles.indexOf(vehicle);
        }
      });
      this.vehicles[index] = vehicleModified;
      this.show_detail = false;
    },

    onCloseForm() {
      this.show_detail = false;
    },

    /* ALMOST COMPUTED VAR */
    formatDate(date) {
      return moment(date).format('MM/DD/YYYY');
    },
  },
}
</script>

<style>
.master-button-group {
  display: inline-block;
  padding-left: 30px;
  padding-right: 0px;
}

.master-button-group button {
  width: 75px;
  margin-left: 5px;
  margin-bottom: 5px;
}

.master {
  padding: 20px;
  margin: 0px;
}

.list {
  -webkit-transition: width 0.5s;
  -moz-transition: width 0.5s;
  -o-transition: width 0.5s;
  transition: width 0.5s;
}

.btn-crear {
  width: 100%;
  margin: 5px 0 5px 0;
}

thead,
tr {
  width: 100%;
  padding: 0 5px 0 5px;
}

td {
  min-width: 130px;
}
</style>
