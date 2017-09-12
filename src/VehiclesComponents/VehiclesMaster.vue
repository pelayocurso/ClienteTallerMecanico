<template>
<div class="master row">
  <div class="list col-sm-12 col-md-6">
    <button class="btn btn-primary btn-crear" @click="handleCreateNewVehicleClick">Crear nuevo vehiculo</button>
    <div class="list-group">
      <a v-for="vehicle in vehicles" class="list-group-item clearfix">
          <span class="col-sm-8">Descripción: {{ vehicle.Id }} --- Fecha: {{ vehicle.Date }}</span>
          <div class="master-button-group col-sm-12 col-md-4">
            <button class="btn btn-default col-12" @click="handleEditClick(vehicle)">Editar</button>
            <button class="btn btn-danger col-12" @click="handleDeleteClick(vehicle)">Borrar</button>
          </div>
        </a>
    </div>
  </div>

  <vehicle @addVehicle="onAddVehicle" @modifyVehicle="onModifyVehicle" :api_host="host"></vehicle>
</div>
</template>

<script>
import Vehicle from './Vehicle.vue';
import axios from 'axios';

export default {
  name: 'vehicle-master',

  components: {
    Vehicle
  },

  data() {
    return {
      vehicles: [],
      host: 'http://localhost:50072/api/Vehicles',
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
        }).catch((error) => {
          Vue.$emit('show-modal', error.message, error.stack)
        });
    },

    deleteVehicle(vehicle) {
      axios.delete(this.host + '/' + vehicle.Id)
        .then((response) => {
          let index = this.vehicles.indexOf(vehicle);
          this.vehicles.splice(index, 1);
        }).catch((error) => {
          console.log(error);
        });
    },

    /* HANDLE SELF EVENTS */
    handleCreateNewVehicleClick() {
      Vue.$emit('show-form', null);
    },

    handleEditClick(vehicle) {
      Vue.$emit('show-form', vehicle);
    },

    handleDeleteClick(vehicle) {
      this.deleteVehicle(vehicle);
    },

    /* HANDLE CHILDREN EVENTS */
    onAddVehicle(vehicle) {
      this.vehicles.push(vehicle);
    },

    onModifyVehicle(vehicleModified) {
      this.vehicles.forEach((vehicle) => {
        if (vehicle.Id == vehicleModified.Id) {
          vehicle = vehicleModified;
        }
      });
    },
  },
}
</script>

<style>
.list-group-item span {
  display: block;
  padding: 10px;
  max-width: 100%;
  word-wrap: break-word;
}

.master-button-group {
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
}

.btn-crear {
  width: 100%;
  margin: 5px 0 5px 0;
}
</style>
