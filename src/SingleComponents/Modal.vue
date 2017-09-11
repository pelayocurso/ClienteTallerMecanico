<template>
<div id="myModal" class="modal fade show" tabindex="-1" role="dialog">
<!-- <div :class="{'modal': true, 'fade': true, 'show': show}"> -->
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title">{{ title }}</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close" @click="closeButton">
            <span aria-hidden="true">&times;</span>
          </button>
      </div>
      <div class="modal-body">
        <p>{{ body }}</p>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-primary" data-dismiss="modal" @click="closeButton">Close</button>
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
      body: ''
    };
  },

  mounted() {
    let _this = this;
    Vue.$on('show-modal', (title, body) => {
      // console.log('on: show-modal');
      _this.title = title;
      _this.body = body;
      $('#myModal').modal('show');
    });
  },

  methods: {
    closeButton() {
      $('#myModal').modal('hide');
    }
  }
};
</script>

<style>
</style>
