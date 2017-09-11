<template>
<div id="app">
  <modal></modal>
  <alert></alert>
  <tab-master></tab-master>
</div>
</template>

<script>
import Modal from './SingleComponents/Modal.vue';
import Alert from './SingleComponents/Alert.vue';
import TabMaster from './TabsComponents/TabMaster.vue';

export default {
  name: 'app',
  components: {
    Modal,
    Alert,
    TabMaster
  }
}
</script>

<style>
</style>
