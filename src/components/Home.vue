<template>
  <div id="home">
    <nav class="navHeader" @click="fromTop">henri.love</nav>
    <nav class="sideNav">
      <ul>
        <li class="heart" v-for="n in projects.length + 1" @click="goToSection(n)"></li>
      </ul>
    </nav>
    <section class="home_section">
        <div class="brand">
          <span class="icon-logo_henrilove"></span>
          <h1>henri.love</h1>
          <h2>étudiant ingénieur créatif</h2>
          <h3>/* aime le monde depuis 1995 */</h3>
        </div>
    </section>
    <section v-for="(project, index) in projects">
      <div class='preview' @click="fromRight()">
        <div>
          <h2>{{project.titre}}</h2>
        </div>
        <div>
          <img :src="project.image" />
        </div>
        <div>
          <h3>/* {{project.date}} - {{project.type}} */</h3>
        </div>
      </div>
    </section>
    <section class="project_info">
      <nav class="return" @click="toRight()">Retour</nav>
      <div class="container">
        <h1>{{projectInfoTitre}}</h1>
        <p>{{projectInfoData}}</p>
        <hr />
        <div class="slider hide">
          <span class="previous" @click="goToImage(slideNumberSaved - 1)">
            <icon name="angle-left"></icon>
          </span>
          <span class="next" @click="goToImage(slideNumberSaved + 1)">
            <icon name="angle-right"></icon>
          </span>
          <img :src="projectInfoCurrentImage" />
          <ul class="slider-list">
            <li class="slider-heart" v-for="(n, index) in projectInfoImages" @click="goToImage(index)"></li>
          </ul>
        </div>
        <p v-for="(item, index) in projectInfoTexte">{{projectInfoTexte[index]}}</p>
        <a v-if="projectInfoType === 'web'" class="link" :href="projectInfoLink">Voir site</a>
        <a v-if="projectInfoType === 'vidéo'" class="link" :href="projectInfoLink">Voir vidéo</a>
      </div>
      <nav class="nextProject" @click="nextProject()">
        <p>Projet suivant : {{projectInfoNext}}</p>
        <img :src="projectImageNext" />
      </nav>
    </section>
  </div>
</template>

<script>
  import Icon from 'vue-awesome/components/Icon'
  import 'vue-awesome/icons/angle-right'
  import 'vue-awesome/icons/angle-left'

  export default {
    name: 'app',
    components: {
      Icon
    },
    data () {
      return {
        projects: [],
        projectInfoTitre: '',
        projectInfoData: '',
        projectInfoImages: [],
        projectInfoCurrentImage: '',
        projectImageNext: '',
        slideNumberSaved: 0,
        projectInfoTexte: [],
        projectInfoType: '',
        projectInfoLink: '',
        projectInfoNext: '',
        direction: '',
        sectionNumber: 0,
        sectionNumberMax: 0,
        sectionNumberSaved: 0,
        projectIndex: 0,
        topValue: 0,
        startY: 0
      }
    },
    methods: {
      goToImage (value) {
        if (value >= 0 && value <= this.projectInfoImages.length - 1) {
          this.projectInfoCurrentImage = this.projectInfoImages[value]
          this.unsetActiveSlide(this.slideNumberSaved)
          this.setActiveSlide(value)
          this.slideNumberSaved = value
        } else if (value >= this.projectInfoImages.length - 1) {
          value = 0
          this.projectInfoCurrentImage = this.projectInfoImages[value]
          this.unsetActiveSlide(this.slideNumberSaved)
          this.setActiveSlide(value)
          this.slideNumberSaved = value
        } else if (value <= 0) {
          value = this.projectInfoImages.length - 1
          this.projectInfoCurrentImage = this.projectInfoImages[value]
          this.unsetActiveSlide(this.slideNumberSaved)
          this.setActiveSlide(value)
          this.slideNumberSaved = value
        }
      },
      setActiveSlide (value) {
        var element = document.getElementsByClassName('slider-heart')[value]
        element.classList.add('isactive')
      },
      unsetActiveSlide (value) {
        var element
        if (document.getElementsByClassName('slider-heart')[value]) {
          element = document.getElementsByClassName('slider-heart')[value]
          element.classList.remove('isactive')
        }
      },
      setActive (value) {
        var element = document.getElementsByClassName('heart')[value]
        element.classList.add('isactive')
      },
      unsetActive (value) {
        var element = document.getElementsByClassName('heart')[value]
        element.classList.remove('isactive')
      },
      goToSection (value) {
        this.sectionNumberSaved = this.sectionNumber
        this.sectionNumber = value - 1
        this.projectIndex = this.sectionNumber - 1
        this.setTopValue(-100 * this.sectionNumber)
        this.unsetActive(this.sectionNumberSaved)
        this.setActive(this.sectionNumber)
      },
      fromTop () {
        document.getElementById('perso').style.top = '0vh'
      },
      fromRight () {
        document.getElementsByClassName('project_info')[0].style.right = '0vw'
        this.buildProjectInfo()
        this.projectInfoCurrentImage = this.projectInfoImages[0]
        setTimeout(function () { this.goToImage(0) }.bind(this), 1)
        document.removeEventListener('DOMMouseScroll', this.detectScrollDirection)
        document.removeEventListener('mousewheel', this.detectScrollDirection)
        document.removeEventListener('touchstart', this.handleMoveStart, false)
        document.removeEventListener('touchend', this.handleMoveEnd, false)
        window.removeEventListener('keyup', this.handleKeyboard, false)
      },
      toRight () {
        document.getElementsByClassName('project_info')[0].style.right = '-100vw'
        document.addEventListener('DOMMouseScroll', this.detectScrollDirection)
        document.addEventListener('mousewheel', this.detectScrollDirection)
        document.addEventListener('touchstart', this.handleMoveStart, false)
        document.addEventListener('touchend', this.handleMoveEnd, false)
        window.addEventListener('keyup', this.handleKeyboard, false)
      },
      buildProjectInfo () {
        this.projectInfoTitre = this.projects[this.projectIndex].titre
        this.projectInfoData = this.projects[this.projectIndex].date + ' - ' +
                               this.projects[this.projectIndex].type + ' - ' +
                               this.projects[this.projectIndex].formation + ' ~ ' +
                               this.projects[this.projectIndex].tag
        this.projectInfoImages = []
        for (var i = 0; i < this.projects[this.projectIndex].illustration.length; i++) {
          this.projectInfoImages[i] = this.projects[this.projectIndex].illustration[i]
        }
        this.projectInfoTexte = [this.projects[this.projectIndex].texte[0], this.projects[this.projectIndex].texte[1], this.projects[this.projectIndex].texte[2]]
        this.projectInfoType = this.projects[this.projectIndex].type
        this.projectInfoLink = this.projects[this.projectIndex].source
        if (this.projectIndex + 1 >= this.sectionNumberMax) {
          this.projectIndex = -1
        }
        this.projectInfoNext = this.projects[this.projectIndex + 1].titre
        this.projectImageNext = this.projects[this.projectIndex + 1].nextimage
      },
      nextProject () {
        this.sectionNumberSaved = this.sectionNumber
        this.toRight()
        setTimeout(function () {
          if (this.sectionNumber === this.sectionNumberMax) {
            this.sectionNumber = 1
          } else {
            this.sectionNumber++
          }
          this.projectIndex = this.sectionNumber - 1
          this.setTopValue(-100 * this.sectionNumber)
          this.unsetActive(this.sectionNumberSaved)
          this.setActive(this.sectionNumber)
        }.bind(this), 500)
        setTimeout(function () {
          this.fromRight()
        }.bind(this), 1100)
      },
      setTopValue (value) {
        var element = document.getElementById('home')
        element.style.top = value + 'vh'
      },
      detectScrollDirection (e) {
        e.preventDefault()
        var delta = null

        if (!e) {
          e = window.event
        }
        if (!e) {
          e = window.event
        }

        if (e.wheelDelta) {
          delta = e.wheelDelta / 60
        } else if (e.detail) {
          delta = -e.detail / 2
        }

        if (delta !== null) {
          this.direction = delta > 0 ? 'up' : 'down'
        }

        this.handleMouseWheelDirection()
      },
      handleMouseWheelDirection () {
        this.sectionNumberSaved = this.sectionNumber
        if (this.direction === 'down') {
          if (this.sectionNumber < this.sectionNumberMax) {
            this.sectionNumber++
            this.projectIndex = this.sectionNumber - 1
          } else {
            this.sectionNumber = 1
            this.projectIndex = this.sectionNumber - 1
          }
        } else if (this.direction === 'up') {
          if (this.sectionNumber > 0) {
            this.sectionNumber--
            if (this.sectionNumber !== 0) {
              this.projectIndex = this.sectionNumber - 1
            }
          }
        } else {
          console.log('Erreur : Direction du scroll non détécté !')
        }
        this.setTopValue(-100 * this.sectionNumber)
        this.unsetActive(this.sectionNumberSaved)
        this.setActive(this.sectionNumber)
      },
      handleMoveStart (e) {
        this.startY = e.changedTouches[0].pageY
      },
      handleMoveEnd (e) {
        var dist = e.changedTouches[0].pageY - this.startY
        console.log('3 ' + dist)

        if (dist < -100) {
          this.direction = 'down'
          this.handleMouseWheelDirection()
        } else if (dist > 100) {
          this.direction = 'up'
          this.handleMouseWheelDirection()
        }
      },
      handleKeyboard (e) {
        switch (e.key) {
          case 'ArrowUp':
            this.direction = 'up'
            this.handleMouseWheelDirection()
            break
          case 'ArrowDown':
            this.direction = 'down'
            this.handleMouseWheelDirection()
            break
        }
      }
    },
    created () {
      document.addEventListener('DOMMouseScroll', this.detectScrollDirection)
      document.addEventListener('mousewheel', this.detectScrollDirection)
      document.addEventListener('touchstart', this.handleMoveStart, false)
      document.addEventListener('touchend', this.handleMoveEnd, false)
      window.addEventListener('keyup', this.handleKeyboard, false)

      this.$http.get('/static/projects.json').then((response) => {
        this.projects = response.data
        this.sectionNumberMax = this.projects.length
      }, (response) => {
        console.log('erreur', response)
      })
    },
    mounted () {
      this.setActive(0)
    }
  }
</script>

<style>
  .fa-icon {
    transform: scale(2);
  }
  section {
    height: 100vh;
    width: 100vw;
    position: relative;
    z-index: 100;
  }

  nav.navHeader {
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

  nav.navHeader:hover {
    border: solid 1px black;
    background-color: white;
    color: black;
  }

  @media screen and (max-width: 768px) and (max-height: 800px) {
    nav.navHeader {
      padding: 5px;
      top: 5px;
      right: 5px;
      font-size: 14px;
    }
  }

  nav.sideNav {
    position: fixed;
    top: 50%;
    right: 26px;
    transform: translateY(-50%);
    z-index: 1000;
  }

  nav.sideNav ul {
    list-style-type:none;
  }

  ul.slider-list {
    list-style-type:none;
    text-align: center;
    padding: 0;
  }

  ul.slider-list li {
    display: inline-block;
    margin: 0 10px;
  }

  .slider-heart {
    position: relative;
    transform: rotate(45deg);
  }

  @media (max-width: 900px) and (orientation: landscape) {
    .slider.hide {
      display: none;
     }
  }

  .heart {
    position: relative;
    margin-top: 30px;
    transform: rotate(45deg);
  }

  @media (max-width: 900px) and (orientation: landscape) {
    nav.sideNav {
      top: 55%;
    }
    .heart { margin-top: 20px; }
  }

  @media (max-width: 600px) and (orientation: landscape) {
    .heart { margin-top: 15px; }
  }

  .slider-heart,
  .slider-heart::before,
  .slider-heart::after,
  .heart,
  .heart::before,
  .heart::after{
    height: 12px;
    width: 12px;
    background-color: black;
    cursor: pointer;
  }

  .slider-heart::before,
  .slider-heart::after,
  .heart::before,
  .heart::after{
    position: absolute;
    content: "";
    border-radius: 4px;
  }

  .slider-heart::before,
  .heart::before{
    left: -6px;
    bottom: 0;
  }

  .slider-heart::after,
  .heart::after{
    left: 0;
    bottom: 6px;
  }

  nav.sideNav .heart.isactive,
  nav.sideNav .heart.isactive::before,
  nav.sideNav .heart.isactive::after {
    background-color: #fff;
    transition: background-color .5s ease-in-out;
  }

  .slider-heart.isactive,
  .slider-heart.isactive::before,
  .slider-heart.isactive::after {
    background-color: #DAA520;
    transition: background-color .5s ease-in-out;
  }

  .heart:hover,
  .heart:hover::before,
  .heart:hover::after {
    background-color: #666;
    transition: background-color .5s ease-in-out;
  }

  .brand {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
  }

  .icon-logo_henrilove,
  .home_section h1,
  .home_section h2,
  .home_section h3 {
    font-family: 'Lucida Console', monospace;
    color: white;
    letter-spacing: -18;
    text-align: center;
    margin: 0 0 10px 0;
  }

  ::selection { background-color: black; }

  .icon-logo_henrilove {
    font-size: 350px;
    display: block;
    text-align: center;
  }

  @media screen and (max-width: 768px) and (max-height: 800px) {
    .icon-logo_henrilove {
      font-size: 200px;
    }
  }

  .home_section h1 {
    font-size: 100px;
    margin: 0;
  }

  @media (max-width: 600px) and (max-height: 1000px) {
    .home_section h1 {
      font-size: 54px;
    }
  }

  @media screen and (max-width: 768px) and (max-height: 800px) {
    .home_section h1 {
      font-size: 40px;
    }
  }

  .home_section h2 {
    font-size: 34px;
  }

  @media screen and (max-width: 700px) and (max-height: 800px) {
    .home_section h2 {
      font-size: 28px;
    }
  }

  @media screen and (max-width: 800px) {
    .home_section h2 {
      font-size: 20px;
    }
  }

  @media screen and (max-width: 768px) and (max-height: 800px) {
    .home_section h2 {
      font-size: 12px;
    }
  }

  .home_section h3 {
    font-size: 25px;
  }

  @media screen and (max-width: 1280px) and (max-height: 800px) {
    .home_section h3 {
      font-size: 18px;
      margin-top: 0px;
    }
  }

  @media screen and (max-width: 800px) {
    .home_section h3 {
      font-size: 18px;
    }
  }

  @media screen and (max-width: 768px) and (max-height: 800px) {
    .home_section h3 {
      font-size: 10px;
    }
  }

  #home {
    position: absolute;
    top: 0vh;
    transition: top .5s cubic-bezier(.5,0,.5,1);
  }

  .preview {
    position: absolute;
    width: 600px;
    top: 50%;
    left: 50%;
    border: solid 5px white;
    transform: translate(-50%,-50%);
    cursor: pointer
  }

  @media (max-width: 700px) and (max-height: 1000px) {
    .preview {
      width: 400px;
     }
  }

  @media (max-width: 900px) and (orientation: landscape) {
    .preview {
      height: 300px;
     }
  }

  @media (max-width: 900px) and (orientation: landscape) {
    .project_info .container {
      width: 250px;
      margin-top: 10px;
    }
  }


  @media screen and (max-width: 768px) and (max-height: 800px) {
    .preview {
      width: 250px;
     }
  }

  @media screen and (max-width: 350px) and (max-height: 800px) {
    .preview {
      width: 200px;
     }
  }

  .preview div:first-child {
    height: 75px;
    border: solid 5px white;
    background-color: black;
  }

  .preview:hover,
  .preview:hover div:first-child,
  .preview:hover div:nth-child(2),
  .preview:hover div:last-child {
    border-color: black;
  }

  .preview div:nth-child(2) {
    height: 200px;
    border: solid 5px white;
    overflow: hidden;
  }

  .preview img {
    filter: invert(0%);
    width: 600px;
    left: 50%;
    transform: translateX(0%);
  }

  .preview:hover img {
    filter: invert(100%);
  }

  @media screen and (max-width: 768px) and (max-height: 800px) {
    .preview img {
      overflow: hidden;
      transform: translateX(-35%);
    }
  }

  .preview div:last-child {
    height: 75px;
    border: solid 5px white;
    background-color: black;
  }

  @media (max-width: 900px) and (orientation: landscape) {
    .preview div:first-child {
      height: 35px;
    }

    .preview div:last-child {
      height: 35px;
    }

    .preview div:nth-child(2) {
      height: 200px;
    }
  }

  .preview h2{
    font-size: 42px;
    margin: 20px 0;
  }

  @media screen and (max-width: 700px) and (max-height: 1000px) {
    .preview h2{
      font-size: 30px;
    }
  }

  @media screen and (max-width: 768px) and (max-height: 800px) {
    .preview h2{
      font-size: 24px;
    }
  }

  @media screen and (max-width: 400px) and (max-height: 800px) {
    .preview h2{
      font-size: 20px;
    }
  }

  @media (max-width: 900px) and (orientation: landscape) {
    .preview h2{
      font-size: 18px;
      margin: 8px 0;
    }
  }

  .preview h3{
    font-size: 14px;
    margin: 30px 0;
  }

  @media screen and (max-width: 400px) and (max-height: 800px) {
    .preview h3{
      font-size: 16px;
      margin: 30px 0;
    }
  }

  @media (max-width: 900px) and (orientation: landscape) {
    .preview h3{
      font-size: 14px;
      margin: 12px 0;
    }
  }

  .preview h2,
  .preview h3 {
    font-family: 'Lucida Console', monospace;
    text-align: center;
    color: #fff;
  }

  .preview:hover div:last-child,
  .preview:hover div:first-child {
    background-color: white;
  }

  .preview:hover h2,
  .preview:hover h3 {
    color: black;
  }

  .preview,
  .preview div:first-child,
  .preview div:nth-child(2),
  .preview div:last-child,
  .preview h3,
  .preview h2,
  .preview img {
    transition: all .5s ease-in-out;
  }

  .preview h2::selection {
    color: white;
  }

  .preview h3::selection {
    color: white;
  }

  .project_info {
    position: fixed;
    right: -100vw;
    top: 0;
    width: 100vw;
    height: 100vh;
    background-color: white;
    z-index: 10000;
    font-family: 'Lucida Console', monospace;

    transition: right .5s cubic-bezier(.04,.06,1,.05);
  }

  @media screen and (max-width: 700px) and (max-height: 1000px) {
    .project_info .container {
      width: 500px;
      margin-top: 40px;
    }
  }

  @media screen and (max-width: 768px) and (max-height: 800px) {
    .project_info .container {
      width: 300px;
      margin-top: 40px;
    }

    .project_info h1 {
      margin: 0;
    }
  }

  @media screen and (max-width: 768px) and (orientation:landscape) {
    .project_info .container {
      width: 400px;
      margin-top: 10px;
    }
  }

  @media screen and (max-width: 1400px) and (max-height: 800px) {
    .project_info p {
      font-size: 12px;
    }
  }

  @media screen and (max-width: 768px) and (max-height: 800px) {
    .project_info p {
      font-size: 14px;
    }
  }

  @media screen and (max-width: 400px) and (max-height: 800px) {
    .project_info p {
      font-size: 13px;
    }
  }

  @media screen and (max-width: 768px) and (orientation:landscape) {
    .project_info p {
      font-size: 12px;
    }
  }

  @media screen and (max-height: 350px) and (orientation:landscape) {
    .project_info p {
      font-size: 11px;
    }
  }

  @media screen and (max-width: 400px) and (max-height: 600px) {
    .project_info p {
      font-size: 9px;
    }
  }

  .project_info img {
    width: 600px;
    margin: 0 auto;
    display: block;
  }

  @media screen and (max-height: 900px) {
    .project_info img {
      width: 350px;
    }
  }

  @media screen and (max-width: 700px) and (max-height: 1000px) {
    .project_info img {
      width: 350px;
    }
  }

  @media screen and (max-width: 1400px) and (max-height: 800px) {
    .project_info img {
      width: 350px;
    }
  }

  @media screen and (max-width: 1000px) and (max-height: 800px) {
    .project_info img {
      width: 200px;
    }
  }

  .project_info h1::selection,
  .project_info p::selection,
  .project_info a::selection,
  .project_info nav::selection {
    color: white;
  }

  a.link,
  nav.return {
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

  @media screen and (max-width: 768px) and (max-height: 800px) {
    nav.return {
      padding: 5px;
      top: 5px;
      right: 5px;
      font-size: 14px;
    }

    a.link {
      font-size: 14px;
      padding: 5px;
    }
  }

  a.link {
    position: static;
    margin-top: 10px;
  }

  a.link:hover,
  nav.return:hover {
    border: solid 1px black;
    background-color: black;
    color: white;
  }

  .previous {
    position: relative;
    top: 210px;
    left: -20px;
  }

  @media screen and (max-height: 900px) {
    .previous {
      top: 130px;
      left: 105px;
    }
  }

  @media screen and (max-width: 700px) and (max-height: 1000px) {
    .previous {
      top: 130px;
      left: 50px;
    }
  }

  @media screen and (max-width: 1400px) and (max-height: 800px) {
    .previous {
      position: relative;
      top: 130px;
      left: 105px;
    }
  }

  @media screen and (max-width: 1000px) and (max-height: 800px) {
    .previous {
      top: 80px;
      left: 180px;
    }
  }

  @media screen and (max-width: 768px) and (max-height: 800px) {
    .previous {
      position: relative;
      top: 80px;
      left: 30px;
    }
  }

  .next {
    position: relative;
    top: 210px;
    right: -600px;
  }

  @media screen and (max-height: 900px) {
    .next {
      top: 130px;
      right: -472px;
    }
  }

  @media screen and (max-width: 700px) and (max-height: 1000px) {
    .next {
      top: 130px;
      right: -428px;
    }
  }

  @media screen and (max-width: 1400px) and (max-height: 800px) {
    .next {
      position: relative;
      top: 130px;
      right: -472px;
    }
  }

  @media screen and (max-width: 1000px) and (max-height: 800px) {
    .next {
      top: 80px;
      right: -400px;
    }
  }

  @media screen and (max-width: 768px) and (max-height: 800px) {
    .next {
      top: 80px;
      right: -250px;
    }
  }

  @media screen and (max-width: 768px) and (max-height: 800px) {
    .next {
      position: relative;
      top: 80px;
      right: -250px;
    }
  }

  .next,
  .previous {
    cursor: pointer;
  }

  .container {
    width: 600px;
    margin: 0 auto;
  }

  .container > p:nth-of-type(4){
    margin-bottom: 30px;
  }

  @media screen and (max-width: 768px) and (max-height: 800px) {
    .container > p:nth-of-type(4){
      margin-bottom: 15px;
    }
  }

  nav.nextProject {
    position: absolute;
    bottom: 0;
    width: 100vw;
    background-color: black;
    height: 100px;
    text-align: center;
    cursor: pointer;
  }

  nav.nextProject p {
    position: absolute;
    color: white;
    top: 0;
    z-index: 100;
    left: 50%;
    transform: translateX(-50%);
    font-size: 24px;
    font-weight: bold;
  }

  nav.nextProject:hover p {
    color: black;
  }

  @media (hover: none) {
    nav.nextProject:hover p {
      color: white;
    }
  }

  nav.nextProject img {
    position: absolute;
    left: 0;
    bottom: 0;
    height: 100%;
    width: 100%;
  }

  @media screen and (max-width: 768px) and (max-height: 800px) {
    nav.nextProject {
      height: 50px;
    }

    nav.nextProject img {
      height: 50px;
      min-width: 500px;
    }

    nav.nextProject p {
      font-size: 14px;
    }
  }

  nav.nextProject:hover img {
    filter: invert(100%);
  }

  @media (hover: none) {
    nav.nextProject:hover img {
      filter: invert(0%);
    }
  }

  nav.nextProject img,
  nav.nextProject p {
    transition: all .5s ease-in-out;
  }
</style>
