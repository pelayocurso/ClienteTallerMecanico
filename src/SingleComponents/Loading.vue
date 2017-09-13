<template>
<div v-if="loading" id="loading">
  <img id="loading-image" src="../assets/img/ajax-loader.gif" alt="Loading..." />
</div>
</template>

<script>
export default {
  name: 'loading',

  data() {
    return {
      loading: false
    }
  },

  created() {
    Vue.$on('show-loading', () => {
      this.loading = true;
    });

    Vue.$on('close-loading', () => {
      this.loading = false;
    });
  },
}
</script>

<style>
#loading {
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  position: fixed;
  display: block;
  opacity: 0.7;
  background-color: #373a3c;
  z-index: 99;
  text-align: center;
}

#loading-image {
  position: fixed;
  top: 50%;
  left: 50%;
  width: auto;
  height: auto;
  -webkit-transform: translate(-50%, -50%);
  -moz-transform: translate(-50%, -50%);
  -ms-transform: translate(-50%, -50%);
  -o-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
}
</style>
