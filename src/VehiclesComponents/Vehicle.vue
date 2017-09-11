
<template>
<div class="detail col-sm-12 col-md-6" v-if="vehicle">
  <form>
    <h3 v-if="vehicle.Id" class="text-center">Modificando Vehiculo</h3>
    <h3 v-else class="text-center">Creando Vehiculo</h3>

    <div class="form-group">
      <label for="owner-vehicle">Dueño: </label>
      <input type="text" class="form-control" id="owner-vehicle" v-model="vehicle.Owner" />
    </div>

    <div class="form-group">
      <label for="type-vehicle">Tipo de Vehiculo: </label>
      <!-- <input type="text" class="form-control" id="type-vehicle" v-model="vehicle.Type" /> -->
      <div id="type-vehicle" class="radio">
        <label>
          <input type="radio" id="moto-vehicle" value="MOTO" v-model="vehicle.Type"/>
          Moto
        </label>
      </div>
      <div id="type-vehicle" class="radio">
        <label>
          <input type="radio" id="moto-vehicle" value="COCHE" v-model="vehicle.Type"/>
          Coche
        </label>
      </div>
      <div id="type-vehicle" class="radio">
        <label>
          <input type="radio" id="moto-vehicle" value="CAMION" v-model="vehicle.Type"/>
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
      <input type="text" class="form-control" id="state-vehicle" v-model="vehicle.State" />
    </div>

    <div class="form-group">
      <label for="budget-vehicle">Presupuesto: </label>
      <input type="number" class="form-control" id="budget-vehicle" v-model="vehicle.Budget" />
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
import DatetimePicker from '../SingleComponents/DatetimePicker.vue';

export default {
  name: 'vehicle',
  props: ['api_host'],

  components: {
    'datetimepicker': DatetimePicker,
  },

  data() {
    return {
      vehicle: null,
      host: this.api_host,
    };
  },

  created() {
    let _this = this;
    Vue.$on('show-form', (vehicle) => {
      if (_this.vehicle) {
        Vue.$emit('refresh-datetimepicker', moment(new Date()).format('MM/DD/YYYY h:mm a'));
      }

      _this.vehicle = vehicle ? vehicle : {
        Owner: '',
        Type: 'COCHE',
        Registration: '',
        Trouble: '',
        State: 'ENTRADA',
        Budget: 1,
        Date: moment(new Date())
      };
    });

    Vue.$on('close-form', () => {
      _this.vehicle = null;
    });

    Vue.$on('edit-vehicle', (vehicle) => {
      if (_this.vehicle) {
        // Vue.$emit('refresh-datetimepicker', moment(vehicle.Fecha).format('MM/DD/YYYY h:mm a'));
        Vue.$emit('refresh-datetimepicker', vehicle.Fecha);
      }
      _this.vehicle = vehicle;
    });
  },

  methods: {
    /* HANDLE SELF EVENTS */
    handleModifyVehicle(event) {
        event.preventDefault();
        // debugger;
        var vehicle = this.vehicle;
        axios.put(this.host + '/' + vehicle.Id, {
            Id: vehicle.Id,
            Fecha: vehicle.Fecha,
            Descripcion: vehicle.Descripcion,
            Tipo: vehicle.Tipo,
            Origen: vehicle.Origen,
            Destino: vehicle.Destino,
            Prioridad: vehicle.Prioridad
          },{headers:{"Content-Type":"application/json"}})
          .then(response=> {
            Vue.$emit('show-modal', 'Vehicle modificado', 'El vehicle ha sido modificado con éxito');
            this.$emit('modifyVehicle', vehicle);
          }).catch((error) => {
            // debugger;
            Vue.$emit('show-modal', error.message, error.stack);
          });

        this.vehicle = null;
    },

    handleCreateVehicle(event) {
        // debugger;
        event.preventDefault();
        var vehicle = this.vehicle;
        if(vehicle.Fecha==""||vehicle.Descripcion==""||vehicle.Tipo==""||vehicle.Origen==""||vehicle.Destino==""||vehicle.Prioridad==""){
        Vue.$emit('show-modal', 'Guardado no permitido', 'Debe rellenar todos los campos antes de poder guardar. Por favor, revíselo');
        }
        else if(vehicle.Prioridad!="Alta"&&vehicle.Prioridad!="Media"&&vehicle.Prioridad!="Baja"){
        Vue.$emit('show-modal', 'Guardado no permitido', 'Los valores aceptador para el campo Prioridad son Alta, Media o Baja');
        }
        else{
          axios.post(this.host, {
            Fecha: vehicle.Fecha,
            Descripcion: vehicle.Descripcion,
            Tipo: vehicle.Tipo,
            Origen: vehicle.Origen,
            Destino: vehicle.Destino,
            Prioridad: vehicle.Prioridad
          })
          .then(response=> {
            Vue.$emit('show-modal', 'Vehicle creado', 'El nuevo vehicle ha sido creado con éxito');
            this.$emit('addVehicle', vehicle);
          });

        }


        this.vehicle = null;
    },

    handleCancel(event) {
      event.preventDefault();
      this.vehicle = null;
    },
  },
}
</script>

<style>
</style>
