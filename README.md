<!DOCTYPE html>
<html>
<head>
  <title>Flappy Macchar</title>
  <style>
    canvas {
      display: block;
      margin: auto;
      background: #70c5ce;
      border: 2px solid #000;
    }
    body {
      text-align: center;
      font-family: sans-serif;
    }
  </style>
</head>
<body>
  <h1>Flappy Macchar</h1>
  <canvas id="gameCanvas" width="320" height="480"></canvas>
  <script src="game.js"></script>
</body>
</html>
const canvas = document.getElementById("gameCanvas");
const ctx = canvas.getContext("2d");

let frames = 0;
const DEGREE = Math.PI / 180;

const state = {
    current: 0,
    getReady: 0,
    game: 1,
    over: 2
};

canvas.addEventListener("click", function () {
    switch (state.current) {
        case state.getReady:
            state.current = state.game;
            break;
        case state.game:
            bird.flap();
            break;
        case state.over:
            location.reload();
            break;
    }
});

const bird = {
    x: 50,
    y: 150,
    radius: 12,
    gravity: 0.25,
    jump: 4.6,
    speed: 0,
    rotation: 0,

    draw: function () {
        ctx.fillStyle = "#000";
        ctx.beginPath();
        ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
        ctx.fill();
    },

    flap: function () {
        this.speed = -this.jump;
    },

    update: function () {
        if (state.current === state.getReady) {
            this.y = 150;
        } else {
            this.speed += this.gravity;
            this.y += this.speed;

            if (this.y + this.radius >= canvas.height) {
                this.y = canvas.height - this.radius;
                if (state.current === state.game) {
                    state.current = state.over;
                }
            }
        }
    }
};

function draw() {
    ctx.fillStyle = "#70c5ce";
    ctx.fillRect(0, 0, canvas.width, canvas.height);
    bird.draw();
}

function update() {
    bird.update();
}

function loop() {
    update();
    draw();
    frames++;
    requestAnimationFrame(loop);
}

loop();
