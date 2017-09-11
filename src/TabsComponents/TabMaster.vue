<template>
<div>
  <ul class="nav nav-tabs">
    <li v-for="tab in tabs" class="nav-item">
      <a class="nav-link" :class="{'active' : tab.activated}" data-toggle="tab" :href="tab.href" @click="selectTab(tab)">{{ tab.name }}</a>
    </li>
  </ul>

  <div class="tab-content">
    <tab name="Vehiculos" :selected="true">
      <vehicles-master></vehicles-master>
    </tab>

    <tab name="Herramientas" :selected="false">
      <!-- <evento-master></evento-master> -->
    </tab>
  </div>
</div>
</template>

<script>
import Tab from './Tab.vue';

import VehiclesMaster from '../VehiclesComponents/VehiclesMaster.vue';
// import ToolsMaster from '../ToolsComponents/ToolsMaster.vue';


export default {
  name: 'tab-master',
  components: {
    Tab,
    VehiclesMaster,
    // ToolsMaster
  },

  data() {
    return {
      tabs: []
    };
  },

  created() {
    this.tabs = this.$children;
  },

  methods: {
    selectTab(selected_tab) {
      this.tabs.forEach((tab) => {
        tab.activated = (tab.href == selected_tab.href);
      });
      Vue.$emit('close-form');
    }
  }
};
</script>
<style>
</style>
