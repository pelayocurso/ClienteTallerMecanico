<template>
<div class='input-group date' ref="datetimepicker" id="datetimepicker">
  <input type='text' class="form-control" v-model="date" @change="update($event)" />
  <span class="input-group-addon">
      <span class="glyphicon glyphicon-calendar"></span>
  </span>
</div>
</template>

<script>
// Import this component
import datePicker from 'vue-bootstrap-datetimepicker';

export default {
  name: 'datetimepicker',
  props: ['value'],

  components: {
      datePicker
  },

  data() {
    return {
      date: this.value,
    };
  },
  mounted() {
    // console.log('mounted');
    // this.date = moment(this.value).format('MM/DD/YYYY h:mm a');
    this.date = this.value;
    $(this.$refs.datetimepicker).datetimepicker()

    Vue.$on('refresh-datetimepicker', (value) => {
      this.date = value;
    });
  },

  methods: {
    update(event) {
      this.$emit('input', event.target.value);
    }
  },

};
</script>
