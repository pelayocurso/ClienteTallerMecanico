<template>
  <div v-show="activated" class="tab-pane fade" :class="{'active in': activated}">
    <slot></slot>
  </div>
</template>

<script>
  export default {
    name: 'tab',
    props: {
      name: false,
      selected: false
    },

    data () {
      return { activated: false };
    },

    mounted() {
      this.activated = this.selected;
    },

    computed: {
      href() {
        return '#' + this.name.toLowerCase().replace(/ /g, '-');
      }
    }
  }
</script>
