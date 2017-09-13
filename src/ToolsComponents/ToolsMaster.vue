<template>
<div class="master row">
  <alert></alert>

  <div class="list" :class="{'col-lg-12' : !show_detail, 'col-lg-6': show_detail}">
    <button class="btn btn-primary btn-crear" @click="handleCreateNewToolClick">Crear nuevo herramienta</button>

    <div class="table-responsive">
      <table class="table table-hover">
        <thead>
          <tr class="tr-row">
            <th>Identificador</th>
            <th>Tipo</th>
            <th>Nombre</th>
            <th>Acciones</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="tool in tools" class="tr-row">
            <td class="td-field"> {{ tool.Id }} </td>
            <td class="td-field"> {{ tool.Type }} </td>
            <td class="td-field"> {{ tool.Name }} </td>
            <td class="td-field master-button-group">
              <button class="btn btn-default" @click="handleEditClick(tool)">Editar</button>
              <button class="btn btn-danger" @click="handleDeleteClick(tool)">Borrar</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>

  <tool @addTool="onAddTool" @modifyTool="onModifyTool" @closeForm="onCloseForm" :api_host="host"></tool>
</div>
</template>

<script>
import Tool from './Tool.vue';
import Alert from '../SingleComponents/Alert.vue';
import axios from 'axios';

export default {
  name: 'tool-master',

  components: {
    Tool,
    Alert
  },

  data() {
    return {
      tools: [],
      host: 'http://localhost:50072/api/Tools',
      show_detail: false,
    };
  },

  created() {
    this.getAllTools();
  },

  methods: {
    /* SERVER REQUESTS */
    getAllTools() {
      Vue.$emit('show-loading');
      axios.get(this.host)
        .then((response) => {
          Vue.$emit('close-loading');
          this.tools = response.data;
          Vue.$emit(
            'show-alert',
            'success',
            "<span class='glyphicons glyphicons-ok'></span> " +
            "Herramientas cargadas satisfactoriamente."
          );
        }).catch((error) => {
          Vue.$emit('close-loading');
          Vue.$emit('show-error-alert', error);
          // Vue.$emit('show-modal', error.message, error.stack);
        });
    },

    deleteTool(tool) {
      Vue.$emit('show-loading');
      axios.delete(this.host + '/' + tool.Id)
        .then((response) => {
          Vue.$emit('close-loading');
          let index = this.tools.indexOf(tool);
          this.tools.splice(index, 1);
          Vue.$emit(
            'show-alert',
            'success',
            "<span class='glyphicons glyphicons-ok'></span> " +
            "Herramienta borrada satisfacoriamente."
          );
        }).catch((error) => {
          Vue.$emit('close-loading');
          Vue.$emit('show-error-alert', error);
          // Vue.$emit('show-modal', error.message, error.stack);
        });
    },

    /* HANDLE SELF EVENTS */
    handleCreateNewToolClick() {
      Vue.$emit('close-alert');
      this.show_detail = true;
      Vue.$emit('show-form', null);
    },

    handleEditClick(tool) {
      Vue.$emit('close-alert');
      this.show_detail = true;
      Vue.$emit('show-form', tool);
    },

    handleDeleteClick(tool) {
      Vue.$emit('close-alert');
      Vue.$emit(
        'show-modal',
        '¿Seguro?',
        "Esta a punto de borrar una herramienta",
        this.deleteTool, tool
      );
    },

    /* HANDLE CHILDREN EVENTS */
    onAddTool(tool) {
      Vue.$emit(
        'show-alert',
        'success',
        "<span class='glyphicons glyphicons-ok'></span>" +
        "Herramienta creada satisfacoriamente."
      );
      this.tools.push(tool);
      this.show_detail = false;
    },

    onModifyTool(toolModified) {
      Vue.$emit(
        'show-alert',
        'success',
        "<span class='glyphicons glyphicons-ok'></span>" +
        "Herramienta modificada satisfacoriamente."
      );
      let index = null;
      this.tools.forEach((tool) => {
        if (tool.Id == toolModified.Id) {
          index = this.tools.indexOf(tool);
        }
      });
      this.tools[index] = toolModified;
      this.show_detail = false;
    },

    onCloseForm() {
      this.show_detail = false;
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
.tr-row {
  width: 100%;
  padding: 0 5px 0 5px;
}

.td-field {
  min-width: 130px;
}
</style>
