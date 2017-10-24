<template>
  <div id="home">
    <Noise />
    <img src="static/img/main-logo-white.png" />
    <h1><span>henri.love</span></h1>
    <h2> étudiant ingénieur créatif</h2>
    <h3>/* aime le monde depuis 1995 */</h3>
  </div>
</template>

<script>
  import Noise from './Noise'

  export default {
    name: 'app',
    components: {
      Noise
    }
  }
</script>

<style>
  img {
    position: absolute;
    z-index: 100;
    top: calc(50% - 120px);
    left: calc(50% + 30px);
    transform: translate(-50%, -50%);
  }

  h1, h2, h3 {
    position: absolute;
    z-index: 100;
    top: 50%;
    left: 50%;
    font-family: 'Lucida Console', Monaco, monospace;
    color: #fff;
    letter-spacing: -18;
  }

  h1 {
    font-size: 100px;
    transform: translate(-50%, -50%);
   }

   h2 {
     font-size: 38px;
     transform: translate(-50%, 250%);
    }

    h3 {
      font-size: 25px;
      transform: translate(-50%, 650%);
     }

    h1::after, h1::before, h1 span::before, h1 span::after {
      content: "henri.love";
      position: absolute;
      display: block;
      height: 100%;
      width: 100%;
      top: 0;
      left: 0;
      opacity: 0.7;
    }

    h1::before {
      z-index: -4;
      color: #ff3333;
      text-shadow: 0 0 20px #ff3333;
      transform: translateY(-3px);
      animation: glitch 2s cubic-bezier(.25, .46, .45, .94) both infinite
    }

    h1::after {
      z-index: -3;
      color: #ffff66;
      text-shadow: 0 0 20px #ffff66;
      transform: translateX(-3px);
      animation: glitch 4s cubic-bezier(.25, .46, .45, .94) both infinite
    }

    h1 span::before {
      z-index: -2;
      color: #33ffff;
      text-shadow: 0 0 20px #33ffff;
      transform: translateY(3px);
      animation: glitch 2s cubic-bezier(.25, .46, .45, .94) reverse infinite
    }

    h1 span::after {
      z-index: -1;
      color: #6633ff;
      text-shadow: 0 0 20px #6633ff;
      transform: translateX(2px);
      animation: glitch 4s cubic-bezier(.25, .46, .45, .94) reverse infinite
    }

    @keyframes glitch {
      0% {
        transform: translate(0)
      }
      20% {
        transform: translate(-2px, 2px)
      }
      40% {
        transform: translate(-2px, -2px)
      }
      60% {
        transform: translate(2px, 2px)
      }
      80% {
        transform: translate(2px, -2px)
      }
      to {
        transform: translate(0)
      }
    }

    #home {
      height: 100vh;
      background-color: #333;
    }
</style>
