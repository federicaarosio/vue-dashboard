<template>
  <div id="content-wrapper">
    <AppAside/>
    <main>
      <AppCard />
      <AppCard />
    </main>
  </div>
</template>

<script>
import AppAside from './components/AppAside.vue';
import AppCard from './components/AppCard.vue';

export default {
  components: {
    AppAside,
    AppCard,
  },
}
</script>

<style lang="">
  
</style>
