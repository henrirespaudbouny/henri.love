<template>
  <div id="app">
    <Noise />
    <ConnectedDots />
    <Perso></Perso>
    <Home></Home>
  </div>
</template>

<script>
  import ConnectedDots from './components/ConnectedDots'
  import Noise from './components/Noise'
  import Home from './components/Home'
  import Perso from './components/Perso.vue'

  export default {
    name: 'app',
    components: {
      ConnectedDots,
      Noise,
      Home,
      Perso
    }
  }
</script>

<style>
  body {
    background-color: #333;
    overflow: hidden;
    margin: 0;
  }

  a {
    color: #fff;
    text-decoration: none;
  }

  .fromup-enter-active,
  .fromup-leave-active {
    top: 0vh !important;
    transition: top 1s ease-in-out;
  }

  .fromup-enter,
  .fromup-leave-to {
    top: -100vh !important;
  }
</style>
