<template>
  <div id="perso">
    <nav @click="toTop">retour</nav>
    <div class="container">
      <h1>henri.love</h1>
      <hr />
      <h2 class="hide">henri.est</h2>
      <p class="hide">Étudiant en troisième année à l'école d'ingénieur Image Multimédia Audiovisuel et Communication (IMAC) et en deuxième année de master cinéma à l'Université Paris-Est Marne-la-Vallée.</p>
      <h2 class="hide">henri.aime</h2>
      <p class="hide">Le cinéma, la musique, les arts numériques et tout ce qui se rattache aux images. J'aime créer, raconter, transmettre des émotions, idées, par l'intermédiaire de mes travaux.</p>
      <h2 class="hide">henri.fait</h2>
      <p class="hide">Des projets numériques. Principalement des sites web et courts-métrages. J'aime réfléchir et concevoir des expériences numériques.</p>
      <h2 class="hide">henri.veut</h2>
      <p class="hide">J'aime le web et l'audiovisuel et aimerais travailler sur des projets narratifs se rapprochant du format webdocumentaire. Mettre en forme une idée.</p>
      <h2>henri.cherche</h2>
      <p>Un stage de 4 à 6 mois à partir d'avril.</p>
      <h2>henri.cv</h2>
      <p><a href="/static/src/henrirespaudbouny_cv.pdf">voir le pdf</a></p>
      <hr/>
      <h1>henrirespaudbouny@gmail.com</h1>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'perso',
    methods: {
      toTop () {
        document.getElementById('perso').style.top = '-100vh'
      }
    }
  }
</script>

<style>
  #perso {
    position: fixed;
    top: -100vh;
    left: 0;
    height: 100vh;
    width: 100vw;
    background-color: #fff;
    z-index: 10000;
    transition: top .5s cubic-bezier(.04,.06,1,.05);
  }

  #perso a::selection,
  #perso nav::selection,
  #perso h1::selection,
  #perso h2::selection,
  #perso p::selection {
    color: white;
  }

  #perso nav {
    position: absolute;
    right: 20px;
    top: 20px;
    z-index: 1000;

    border: solid 1px black;
    padding: 10px;

    color: #fff;
    font-size: 20px;
    font-family: 'Lucida Console', monospace;

    color: #000;
    cursor: pointer;

    transition: all .5s ease-in-out;
  }

  @media (max-width: 800px) and (orientation: landscape) {
    .hide {display: none; }
  }

  #perso nav:hover {
    border: solid 1px black;
    background-color: black;
    color: white;
  }

  @media screen and (max-width: 500px) and (max-height: 800px) {
    #perso nav {
      top: 5px;
      right: 5px;
      padding: 5px;
      font-size: 14px;
    }
  }

  #perso .container {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 600px;
    transform: translate(-50%,-50%);
   }

   @media (max-width: 700px) and (max-height: 1000px) {
     #perso .container {
       width: 500px;
      }
   }

   @media screen and (max-width: 500px) and (max-height: 800px) {
     #perso .container {
       position: absolute;
       top: 50%;
       left: 50%;
       width: 300px;
       transform: translate(-50%,-50%);
      }
   }

   @media (max-width: 600px) and (orientation: landscape) {
     #perso .container { width: 400px; }
   }

  #perso h1 {
    color: #000;
    font-family: 'Lucida Console', monospace;
  }

  @media screen and (max-width: 500px) and (max-height: 800px) {
    #perso h1 {
      font-size: 22px;
     }
  }

  #perso h1::before,
  #perso h1::after { content: ""; }

  #perso h2 {
    margin: 0px 0;
    font-family: 'Lucida Console', monospace;
  }

  @media screen and (max-width: 768px) and (max-height: 800px) {
    #perso h2 {
      font-size: 18px;
     }
  }

  #perso p {
    margin: 5px 0px 30px 0px;
    font-family: 'Lucida Console', monospace;
  }

  @media screen and (max-width: 1400px) and (max-height: 800px) {
    #perso p {
      margin: 5px 0px 15px 0px;
      font-size: 12px;
    }
  }

  @media screen and (max-width: 1000px) and (max-height: 800px) {
    .next {
      top: 80px;
      right: -395px;
    }
  }

  @media screen and (max-width: 768px) and (max-height: 800px) {
    #perso p {
      font-size: 12px;
    }
  }

  @media screen and (max-width: 400px) and (max-height: 800px) {
    #perso p {
      margin: 5px 0px 15px 0px;
      font-size: 12px;
    }
  }

  @media screen and (max-width: 400px) and (max-height: 600px) {
    #perso p {
      font-size: 10px;
    }
  }

  #perso a {
    color: black;
    font-family: 'Lucida Console', monospace;
    cursor: pointer;
  }

  @media screen and (max-width: 500px) and (max-height: 800px) {
    #perso a {
      font-size: 12px;
     }
  }

  #perso a:hover {
    text-decoration: underline;
  }


</style>
