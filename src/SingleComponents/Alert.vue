<template>
<div v-if="show" class="alert alert-dissmisible show" :class="{
      'alert-success': this.type == 'success',
      'alert-info': this.type == 'info',
      'alert-warning': this.type == 'warning',
      'alert-danger': this.type == 'danger',
      'fade' : !show
    }">
  <button type="button" class="close" arial-label="close" @click="handleCloseAlertClick">
      <span aria-hidden="true">&times;</span>
    </button>

  <div v-html="body">
  </div>
  <!-- {{ escape(body) }} -->
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
    Vue.$on('show-alert', (type, body) => {
      this.type = type;
      this.body = body;
      this.show = true;
    });

    Vue.$on('show-error-alert', (error) => {
      this.showError(error);
    });

    Vue.$on('close-alert', () => {
      this.handleCloseAlertClick();
    });
  },

  methods: {
    /* HANDLE SELF EVENTS */
    handleCloseAlertClick() {
      this.body = null;
      this.show = false;
    },

    /* ISOLATION OF METHOD TO BE CALLABLE FROM REF */
    showError(error) {
      let errors = "<strong>" + error.message + "</strong>";

      if(error.response){
        let model = error.response.data.ModelState;

        if (error.response.status == 400) {
          for (let property in model) {
            if (model.hasOwnProperty(property)) {
              for (let i = 0; i < model[property].length; i++) {
                errors += ("<br/>" + model[property][i]);
              }
            }
          }
        }
      } else {
        errors += "<br />Ocurrio un error al intentar conectar con el servidor."
      }
      this.type = 'danger';
      this.body = errors;
      this.show = true;
    }
  }
}
</script>
