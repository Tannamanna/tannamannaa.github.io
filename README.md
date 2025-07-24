# tannamannaa.github.io
anistvis&lt;3
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>For Ani 💖</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');
  body {
    margin: 0; 
    background: #000;
    overflow: hidden;
    font-family: 'Press Start 2P', cursive;
    color: #fff;
    text-align: center;
  }
  h1 {
    margin-top: 2rem;
    font-size: 2rem;
    text-shadow:
      0 0 5px #f0f,
      0 0 10px #f0f,
      0 0 20px #f0f,
      0 0 40px #f0f;
  }
  .hearts {
    position: fixed;
    top: 0; left: 0; right: 0; bottom: 0;
    pointer-events: none;
    z-index: 10;
  }
  .heart {
    position: absolute;
    width: 20px;
    height: 20px;
    background: #ff69b4;
    clip-path: polygon(
      50% 0%, 61% 10%, 70% 18%, 79% 26%, 85% 35%, 
      90% 45%, 92% 55%, 92% 65%, 88% 75%, 80% 85%, 
      70% 90%, 60% 95%, 50% 100%, 40% 95%, 30% 90%, 
      20% 85%, 12% 75%, 8% 65%, 8% 55%, 10% 45%, 
      15% 35%, 21% 26%, 30% 18%, 39% 10%
    );
    animation: floatUp 4s linear forwards;
    opacity: 0.8;
  }
  @keyframes floatUp {
    0% {
      transform: translateY(0) scale(1);
      opacity: 0.8;
    }
    100% {
      transform: translateY(-200px) scale(0.5);
      opacity: 0;
    }
  }
  canvas {
    position: fixed;
    top:0; left:0;
    width: 100vw;
    height: 100vh;
    pointer-events: none;
    z-index: 5;
  }
</style>
</head>
<body>

<h1>For Ani 💖</h1>

<div class="hearts"></div>
<canvas id="fireworks"></canvas>

<script>
// Hearts animation
const heartsContainer = document.querySelector('.hearts');

function createHeart() {
  const heart = document.createElement('div');
  heart.classList.add('heart');
  heart.style.left = Math.random() * window.innerWidth + 'px';
  heart.style.animationDuration = 3 + Math.random() * 2 + 's';
  heartsContainer.appendChild(heart);

  setTimeout(() => {
    heart.remove();
  }, 4000);
}

setInterval(createHeart, 400);

// Fireworks canvas animation
const canvas = document.getElementById('fireworks');
const ctx = canvas.getContext('2d');
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

const fireworks = [];
const particles = [];

function random(min, max) {
  return Math.random() * (max - min) + min;
}

class Firework {
  constructor() {
    this.x = random(100, canvas.width - 100);
    this.y = canvas.height;
    this.targetY = random(canvas.height/4, canvas.height/2);
    this.speed = random(5, 7);
    this.radius = 2;
    this.color = `hsl(${Math.floor(random(0, 360))}, 100%, 60%)`;
    this.exploded = false;
  }
  update() {
    if (!this.exploded) {
      this.y -= this.speed;
      if (this.y <= this.targetY) {
        this.exploded = true;
        this.explode();
      }
    }
  }
  draw() {
    if (!this.exploded) {
      ctx.beginPath();
      ctx.arc(this.x, this.y, this.radius, 0, Math.PI*2);
      ctx.fillStyle = this.color;
      ctx.fill();
    }
  }
  explode() {
    for(let i=0; i<30; i++) {
      particles.push(new Particle(this.x, this.y, this.color));
    }
  }
}

class Particle {
  constructor(x, y, color) {
    this.x = x;
    this.y = y;
    this.radius = 2;
    this.color = color;
    this.speedX = random(-5, 5);
    this.speedY = random(-5, 5);
    this.alpha = 1;
    this.decay = 0.02;
  }
  update() {
    this.x += this.speedX;
    this.y += this.speedY;
    this.alpha -= this.decay;
  }
  draw() {
    ctx.save();
    ctx.globalAlpha = this.alpha > 0 ? this.alpha : 0;
    ctx.beginPath();
    ctx.arc(this.x, this.y, this.radius, 0, Math.PI*2);
    ctx.fillStyle = this.color;
    ctx.fill();
    ctx.restore();
  }
}

function loop() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  if (fireworks.length < 5) {
    fireworks.push(new Firework());
  }

  fireworks.forEach((fw, i) => {
    fw.update();
    fw.draw();
    if (fw.exploded) fireworks.splice(i, 1);
  });

  particles.forEach((p, i) => {
    p.update();
    p.draw();
    if (p.alpha <= 0) particles.splice(i, 1);
  });

  requestAnimationFrame(loop);
}

loop();

window.addEventListener('resize', () => {
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
});
</script>

</body>
</html>
