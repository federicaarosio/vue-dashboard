<template>
  <div id="content-wrapper">
    <div class="container-fluid">
      <div class="row">
        <AppAside class="col-2" />
        <AppMain class="col-10"/>
      </div>
    </div>
  </div>
</template>

<script>
import AppAside from './components/AppAside.vue';
import AppMain from './components/AppMain.vue';

export default {
  components: {
    AppAside,
    AppMain,
  },
}
</script>

<style lang="">
  
</style>
