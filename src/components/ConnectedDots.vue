<template>
  <canvas id="canvas"  class="connected_dots" resize style="width: 100%; height: 100%"></canvas>
</template>

<script type="text/javascript">
/* eslint-disable */

var canvasbody = document.getElementById("canvas");
var drawarea;
var w;
var h;
var particles = [];

let playExp = false

//SETUP
function mySetup() {
  canvasbody = document.getElementById("canvas");
  drawarea = canvasbody.getContext("2d");
  w = canvasbody.width = window.innerWidth;
  h = canvasbody.height = window.innerHeight;

  for(let i = 0; i < window.innerHeight/100+1; i++) {
    for (let ii = 0; ii < opts.particleAmount; ii++){
      particles.push( new Particle(ii*100, i*100) );
    }
  }
  window.requestAnimationFrame(loop);
}

function mySetupOnResize() {
  canvasbody = document.getElementById("canvas");
  drawarea = canvasbody.getContext("2d");
  w = canvasbody.width = window.innerWidth;
  h = canvasbody.height = window.innerHeight;

  for(let i = 0; i < window.innerHeight/100+1; i++) {
    for (let ii = 0; ii < window.innerWidth/100+1; ii++){
      particles.push( new Particle(ii*100, i*100) );
    }
  }
}

function clearSetup () {
  canvasbody = null
  particles = []
}

window.addEventListener("resize", function(){
    clearSetup ();
    mySetupOnResize();
});

const opts = {
  particleColor: "rgb(100,100,100)",
  lineColor: "rgba(100,100,100)",
  particleAmount: window.innerWidth/100+1,
  defaultSpeed: 2,
  variantSpeed: 1,
  defaultRadius: 1,
  variantRadius: 4,
  linkRadius: 200,
};

var mouseX = 0
var mouseY = 0
var mouseRadius = 50

window.addEventListener('mousemove', function(evt) {
  mouseX = evt.pageX
  mouseY = evt.pageY
})

let checkDistance = function(x1, y1, x2, y2){
  return Math.sqrt(Math.pow(x2 - x1, 2) + Math.pow(y2 - y1, 2));
};

let linkPoints = function(point1, hubs){
  for (let i = 0; i < hubs.length; i++) {
    let distance = checkDistance(point1.x, point1.y, hubs[i].x, hubs[i].y);
    let opacity = 1 - distance / opts.linkRadius;
    if (opacity > 0) {
      drawarea.lineWidth = 0.5;
      drawarea.strokeStyle = `rgba(${rgb[0]}, ${rgb[1]}, ${rgb[2]}, ${opacity})`;
      drawarea.beginPath();
      drawarea.moveTo(point1.x, point1.y);
      drawarea.lineTo(hubs[i].x, hubs[i].y);
      drawarea.closePath();
      drawarea.stroke();
    }
  }
}

let Particle = function(xPos, yPos){
  this.xSave = xPos;
  this.ySave = yPos;
  this.x = xPos;
  this.y = yPos;
  this.speed = opts.defaultSpeed + Math.random() * opts.variantSpeed;
  this.directionAngle = Math.floor(Math.random() * 360);
  this.color = opts.particleColor;
  this.radius = opts.defaultRadius + Math.random() * opts. variantRadius;
  this.vector = {
    x: Math.cos(this.directionAngle) * this.speed/4,
    y: Math.sin(this.directionAngle) * this.speed/4
  };

  this.update = function(){
    if(mouseX == 0) {mouseX = 0.0001}
    if(mouseX == 0) {mouseY = 0.0001}

    if (  this.x/(mouseX) < 1.1 && this.x/(mouseX) > 0.9
       && this.y/(mouseY) < 1.1 && this.y/(mouseY) > 0.9) {
      //this.color = "rgb(255,255,255)";
      if(this.xSave-mouseX < mouseRadius && this.xSave-mouseX > -mouseRadius
        && this.ySave-mouseY < mouseRadius && this.ySave-mouseY > -mouseRadius ) {
        this.x = mouseX;
        this.y = mouseY;
      }
    } else {
      this.speed = opts.defaultSpeed + Math.random() * opts.variantSpeed;
      this.x += this.vector.x;
      this.y += this.vector.y;
    }
    this.border();
  };
  this.border = function(){
    if(this.xSave-this.x > mouseRadius || this.xSave-this.x < -mouseRadius ) {
      this.vector.x *= -1;
    }
    if (this.ySave-this.y > mouseRadius || this.ySave-this.y < -mouseRadius ) {
      this.vector.y *= -1;
    }
  };
  this.draw = function(){
    drawarea.beginPath();
    drawarea.arc(this.x, this.y, this.radius, 0, Math.PI*2);
    drawarea.closePath();
    drawarea.fillStyle = this.color;
    drawarea.fill();
  };
};

function loop(){
  if (playExp) {
    window.requestAnimationFrame(loop);
    drawarea.clearRect(0,0,w,h);
    for (let i = 0; i < particles.length; i++){
      particles[i].update();
      particles[i].draw();
    }
    for (let i = 0; i < particles.length; i++){
      linkPoints(particles[i], particles);
    }
  }
}


let delay = 200, tid,
  rgb = opts.lineColor.match(/\d+/g);

import $ from 'jquery';
export default {
  name: 'ConnectedDots',
  mounted () {
    playExp = true
    mySetup();
  },
  destroyed () {
    playExp = false
    clearSetup()
  }
}
</script>

<style>
  .connected_dots { position: fixed; }
</style>
