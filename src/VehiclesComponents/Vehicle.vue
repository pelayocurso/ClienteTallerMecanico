
<template>
<div v-if="vehicle" class="detail col-sm-12 col-md-6" id="detail">
  <alert ref="vehicleAlert"></alert>
  <form>
    <h3 v-if="vehicle.Id" class="text-center">Modificando Vehiculo</h3>
    <h3 v-else class="text-center">Creando Vehiculo</h3>

    <div class="form-group">
      <label for="owner-vehicle">Dueño: </label>
      <input type="text" class="form-control" id="owner-vehicle" v-model="vehicle.Owner" />
    </div>

    <div class="form-group">
      <label for="type-vehicle">Tipo de Vehiculo: </label>
      <div id="type-vehicle" class="radio">
        <label>
          <input type="radio" id="moto-vehicle" value="0" v-model="vehicle.Type"/>
          Moto
        </label>
      </div>
      <div id="type-vehicle" class="radio">
        <label>
          <input type="radio" id="moto-vehicle" value="1" v-model="vehicle.Type"/>
          Coche
        </label>
      </div>
      <div id="type-vehicle" class="radio">
        <label>
          <input type="radio" id="moto-vehicle" value="2" v-model="vehicle.Type"/>
          Camion
        </label>
      </div>
    </div>

    <div class="form-group">
      <label for="registration-vehicle">Matricula: </label>
      <input type="text" class="form-control" id="registration-vehicle" v-model="vehicle.Registration" />
    </div>

    <div class="form-group">
      <label for="trouble-vehicle">Averia: </label>
      <input type="text" class="form-control" id="trouble-vehicle" v-model="vehicle.Trouble" />
    </div>

    <div class="form-group">
      <label for="state-vehicle">Estado: </label>
      <select class="form-control" id="state-vehicle" v-model="vehicle.State">
        <option value="0">ESPERA</option>
        <option value="1">ENTRADA</option>
        <option value="2">REPARACIÓN</option>
        <option value="3">SALIDA</option>
        <option value="4">HECHO</option>
      </select>
    </div>

    <div class="form-group">
      <label for="budget-vehicle">Presupuesto(€): </label>
      <input type="number" class="form-control" id="budget-vehicle" v-model="vehicle.Budget" step="any" />
    </div>

    <div class="form-group">
      <label for="date-vehicle">Fecha de Entrada: </label>
      <datetimepicker v-model="vehicle.Date"></datetimepicker>
    </div>

    <div class="group-btn">
      <button v-if="vehicle.Id" class="btn btn-success" @click="handleModifyVehicle($event)">Modificar</button>
      <button v-else class="btn btn-success" @click="handleCreateVehicle($event)">Crear</button>
      <button class="btn btn-secondary" @click="handleCancel($event)">Cancelar</button>
    </div>
  </form>
</div>
</template>

<script>
import axios from 'axios';
import Alert from '../SingleComponents/Alert.vue';
import DatetimePicker from '../SingleComponents/DatetimePicker.vue';

export default {
  name: 'vehicle',
  props: ['api_host'],

  components: {
    'datetimepicker': DatetimePicker,
    'alert': Alert
  },

  data() {
    return {
      vehicle: null,
      host: this.api_host,
    };
  },

  created() {
    /* HANDLE OTHER COMPONENTS EVENTS */
    Vue.$on('show-form', (vehicle) => {
      if (this.vehicle) {
        Vue.$emit(
          'refresh-datetimepicker',
          moment(new Date()).format('MM/DD/YYYY h:mm a')
        );
      }

      if (vehicle) {
        this.vehicle = JSON.parse(JSON.stringify(vehicle));
        this.vehicle.Date = moment(this.vehicle.Date)
          .format('MM/DD/YYYY h:mm a');
      } else {
        this.vehicle = {
          Owner: '',
          Type: '1',
          Registration: '',
          Trouble: '',
          State: '0',
          Budget: 1,
          Date: moment(new Date()).format('MM/DD/YYYY h:mm a')
        };
      }
    });

    Vue.$on('close-form', () => {
      this.$parent.show_detail = false;
      this.vehicle = null;
    });
  },

  methods: {
    /* SERVER REQUESTS */
    updateVehicle(vehicle) {
      Vue.$emit('show-loading');
      axios.put(this.host + '/' + vehicle.Id, vehicle)
        .then((response) => {
          Vue.$emit('close-loading');
          this.$emit('modifyVehicle', vehicle);
          this.vehicle = null;
        })
        .catch((error) => {
          Vue.$emit('close-loading');
          this.$refs.vehicleAlert.showError(error);
        });
    },

    createVehicle() {
      Vue.$emit('show-loading');
      axios.post(this.host, this.vehicle)
        .then(response => {
          Vue.$emit('close-loading');
          this.$emit('addVehicle', response.data);
          this.vehicle = null;
        })
        .catch((error) => {
          Vue.$emit('close-loading');
          this.$refs.vehicleAlert.showError(error);
        });
    },

    /* HANDLE SELF EVENTS */
    handleModifyVehicle(event) {
      event.preventDefault();
      Vue.$emit(
        'show-modal',
        'Esta a punto de modificar un vehiculo',
        "¿Esta seguro de que desea continuar?",
        this.updateVehicle, this.vehicle
      );
    },

    handleCreateVehicle(event) {
      event.preventDefault();
      this.createVehicle();
    },

    handleCancel(event) {
      event.preventDefault();
      this.$emit('closeForm');
      this.vehicle = null;
    },
  },
}
</script>

<style>
</style>
