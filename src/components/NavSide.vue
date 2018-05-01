<template>
  <div id="navheader">
    <nav @click="fromTop">henri.love</nav>
  </div>
</template>

<script>
  export default {
    name: 'navheader',
    methods: {
      fromTop () {
        document.getElementById('perso').style.top = '0vh'
      }
    }
  }
</script>

<style>
  #navheader nav {
    border: solid 1px white;
    padding: 10px;
    position: fixed;
    right: 20px;
    top: 20px;
    z-index: 1000;

    color: #fff;
    font-size: 20px;
    font-family: 'Lucida Console', monospace;

    cursor: pointer;

    transition: all .5s ease-in-out;
  }

  #navheader nav:hover {
    border: solid 1px black;
    background-color: white;
    color: black;
  }
</style>
