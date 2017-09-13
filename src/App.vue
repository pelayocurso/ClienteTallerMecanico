<template>
<div id="app">
  <modal></modal>
  <loading></loading>
  <tab-master></tab-master>
</div>
</template>

<script>
import Modal from './SingleComponents/Modal.vue';
import Loading from './SingleComponents/Loading.vue';
import TabMaster from './TabsComponents/TabMaster.vue';

export default {
  name: 'app',
  components: {
    Modal,
    Loading,
    TabMaster
  }
}
</script>

<style>
</style>
