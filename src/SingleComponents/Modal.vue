<template>
<div id="myModal" class="modal fade show" tabindex="-1" role="dialog">
  <!-- <div :class="{'modal': true, 'fade': true, 'show': show}"> -->
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title">{{ title }}</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close" @click="handleCloseButton">
            <span aria-hidden="true">&times;</span>
          </button>
      </div>

      <div class="modal-body">
        <p>{{ body }}</p>
      </div>

      <div v-if="!callback" class="modal-footer">
        <button type="button" class="btn btn-primary" data-dismiss="modal" @click="handleCloseButton">Close</button>
      </div>
      <div v-else class="modal-footer">
        <button type="button" class="btn btn-primary" id="modal-btn-si" @click="handleConfirmClick">Continuar</button>
        <button type="button" class="btn btn-default" id="modal-btn-no" data-dismiss="modal" @click="handleCloseButton">Cancelar</button>
      </div>

    </div>
  </div>
</div>
</template>

<script>
export default {
  name: 'modal',

  data() {
    return {
      title: '',
      body: '',
      callback: null,
      param: null
    };
  },

  mounted() {
    Vue.$on('show-modal', (title, body, callback, param) => {
      this.title = title;
      this.body = body;
      this.callback = callback;
      this.param = param;
      $('#myModal').modal('show');
    });
  },

  methods: {
    handleCloseButton() {
      $('#myModal').modal('hide');
    },

    handleConfirmClick() {
      // Vue.$emit('modal-confirm');
      if (this.param) {
        this.callback(this.param);
      } else {
        this.callback();
      }
      $('#myModal').modal('hide');
    },
  }
};
</script>

<style>
</style>
