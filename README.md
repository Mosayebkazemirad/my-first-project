<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Pong Game</title>
<style>
  html, body {
    margin: 0;
    overflow: hidden;
  }
  canvas {
    display: block;
    background: black;
  }
</style>
</head>
<body>

<canvas id="game"></canvas>

<script>
const canvas = document.getElementById("game")
const ctx = canvas.getContext("2d")

canvas.width = window.innerWidth
canvas.height = window.innerHeight

let ball = {
  x: canvas.width /2 ,
   y: canvas.height /2,
  r: 10,
  dx: -5,
    dy: 4,
  color: "white"
}


const wall = {
  x: 0,
  width: 20
}

// ---------- دروازه‌بان راست ----------
let paddle = {
  width: 15,
  height: 120,
  x: canvas.width - 10,
  y: canvas.height / 4 - 30,
  speed: 10
}

// ---------- کنترل کیبورد ----------
let up = false
let down = false

document.addEventListener("keydown", e => {
  if (e.key === "ArrowUp") up = true
  if (e.key === "ArrowDown") down = true
})

document.addEventListener("keyup", e => {
  if (e.key === "ArrowUp") up = false
  if (e.key === "ArrowDown") down = false
})

// ---------- حلقه بازی ----------
function gameLoop() {
  ctx.clearRect(0, 0, canvas.width, canvas.height)

  // حرکت دروازه‌بان
  if (up && paddle.y > 0) paddle.y -= paddle.speed
  if (down && paddle.y + paddle.height < canvas.height) paddle.y += paddle.speed

  // حرکت توپ
  ball.x += ball.dx
  ball.y += ball.dy

  // برخورد توپ با بالا و پایین
  if (ball.y - ball.r < 0 || ball.y + ball.r > canvas.height) {
    ball.dy *= -1
  }

  // برخورد با دیوار چپ
  if (ball.x - ball.r < wall.width) {
    ball.dx *= -1
  }

  // برخورد با دروازه‌بان
  if (
    ball.x + ball.r > paddle.x &&
    ball.y > paddle.y &&
    ball.y < paddle.y + paddle.height
  ) {
    ball.dx *= -1

    // تغییر زاویه بر اساس محل برخورد
    let hitPoint = (ball.y - paddle.y) / paddle.height
    ball.dy = (hitPoint - 1) * 10
  }

  // گل خوردن
  if (ball.x > canvas.width) {
    resetBall()
  }

  // رسم دیوار
  ctx.fillStyle = "gray"
  ctx.fillRect(0, 0, wall.width, canvas.height)

  // رسم دروازه‌بان
  ctx.fillStyle = "white"
  ctx.fillRect(paddle.x, paddle.y, paddle.width, paddle.height)

  // رسم توپ
  ctx.beginPath()
  ctx.arc(ball.x, ball.y, ball.r, 0, Math.PI * 2)
  ctx.fillStyle = ball.color
  ctx.fill()
  ctx.closePath()

  requestAnimationFrame(gameLoop)
}

function resetBall() {
  ball.x = canvas.width / 2
  ball.y = canvas.height / 2
  ball.dx = -5
  ball.dy = (Math.random() * 6) - 3
}

gameLoop()
</script>

</body>
</html>
