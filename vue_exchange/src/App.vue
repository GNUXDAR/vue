<template>
<!-- <div id="app"> -->
  <main>
    <art-header />

    <router-view class="cointainer px-5 sm:px-20 flex justify-center" />
  </main>
<!-- </div> -->
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
import ArtHeader from '@/components/ArtHeader.vue';

export default {
  name: 'App',
  components: {
    ArtHeader,
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
