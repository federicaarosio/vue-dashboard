<template>
  <div id="content-wrapper">
    <AppAside/>
    <AppMain />
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
