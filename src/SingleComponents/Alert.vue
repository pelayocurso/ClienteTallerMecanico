<template>
  <div v-if="show" class="alert alert-dissmisible fade show" :class="{'alert-succes': this.type == 'success', 'alert-info': this.type == 'info', 'alert-warning': this.type == 'warning', 'alert-danger': this.type == 'danger'}">
    <button type="button" class="close" data-dismiss="alert" arial-label="close">
      <span aria-hidden="true">&times;</span>
    </button>

    <strong v-if="this.type == 'success'">Success!</strong>
    <strong v-elseif="this.type == 'info'">Heads up!</strong>
    <strong v-elseif="this.type == 'warning'">Warning!</strong>
    <strong v-elseif="this.type == 'danger'">Oh snap!</strong>
    {{ body }}
  </div>
</template>

<script>
  export default {
    name: 'alert',

    data() {
      return {
        type: 'info',
        body: null,
        show: false
      };
    },

    created() {
      let _this = this;
      Vue.$on('show-alert', (type, body) => {
        _this.type = type;
        _this.body = body;
        _this.show = true;
      });

      Vue.$on('close-alert', () => {
        _this.body = null;
        _this.show = false;
      });
    }
  }
</script>
