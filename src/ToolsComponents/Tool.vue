
<template>
<div v-if="tool" class="detail col-sm-12 col-md-6" id="detail">
  <alert ref="toolAlert"></alert>
  <form>
    <h3 v-if="tool.Id" class="text-center">Modificando Herramienta</h3>
    <h3 v-else class="text-center">Creando Herramienta</h3>

    <div class="form-group">
      <label for="type-tool">Tipo: </label>
      <input type="text" class="form-control" id="type-tool" v-model="tool.Type" />
    </div>

    <div class="form-group">
      <label for="name-tool">Nombre: </label>
      <input type="text" class="form-control" id="name-tool" v-model="tool.Name" />
    </div>

    <div class="form-group">
      <label for="quantity-tool">Cantidad: </label>
      <input type="number" class="form-control" id="quantity-tool" v-model="tool.Quantity" step="1" />
    </div>

    <div class="form-group">
      <label for="date-tool">Fecha de Entrada: </label>
      <datetimepicker v-model="tool.Date"></datetimepicker>
    </div>

    <div class="group-btn">
      <button v-if="tool.Id" class="btn btn-success" @click="handleModifyTool($event)">Modificar</button>
      <button v-else class="btn btn-success" @click="handleCreateTool($event)">Crear</button>
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
  name: 'tool',
  props: ['api_host'],

  components: {
    'datetimepicker': DatetimePicker,
    'alert': Alert
  },

  data() {
    return {
      tool: null,
      host: this.api_host,
    };
  },

  created() {
    /* HANDLE OTHER COMPONENTS EVENTS */
    Vue.$on('show-form', (tool) => {
      if (this.tool) {
        Vue.$emit(
          'refresh-datetimepicker',
          moment(new Date()).format('MM/DD/YYYY h:mm a')
        );
      }

      if (tool) {
        this.tool = JSON.parse(JSON.stringify(tool));
        this.tool.Date = moment(this.tool.Date)
          .format('MM/DD/YYYY h:mm a');
      } else {
        this.tool = {
          Type: '',
          Name: '',
          Quantity: '1',
          Date: moment(new Date()).format('MM/DD/YYYY h:mm a')
        };
      }
    });

    Vue.$on('close-form', () => {
      this.tool = null;
      this.$parent.show_detail = false;
    });
  },

  methods: {
    /* SERVER REQUESTS */
    updateTool(tool) {
      Vue.$emit('show-loading');
      axios.put(this.host + '/' + tool.Id, tool)
        .then((response) => {
          Vue.$emit('close-loading');
          this.$emit('modifyTool', tool);
          this.tool = null;
        })
        .catch((error) => {
          Vue.$emit('close-loading');
          this.$refs.toolAlert.showError(error);
        });
    },

    createTool() {
      Vue.$emit('show-loading');
      axios.post(this.host, this.tool)
        .then(response => {
          Vue.$emit('close-loading');
          this.$emit('addTool', response.data);
          this.tool = null;
        })
        .catch((error) => {
          Vue.$emit('close-loading');
          this.$refs.toolAlert.showError(error);
        });
    },

    /* HANDLE SELF EVENTS */
    handleModifyTool(event) {
      event.preventDefault();
      Vue.$emit(
        'show-modal',
        'Esta a punto de modificar una herramienta',
        "¿Esta seguro de que desea continuar?",
        this.updateTool, this.tool
      );
    },

    handleCreateTool(event) {
      event.preventDefault();
      this.createTool();
    },

    handleCancel(event) {
      event.preventDefault();
      this.$emit('closeForm');
      this.tool = null;
    },
  },
}
</script>

<style>
</style>
