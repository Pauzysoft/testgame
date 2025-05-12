<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8">
  <title>Uçak Savaşı Oyunu</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    html, body {
      width: 100%;
      height: 100%;
      overflow: hidden;
      background: black;
      font-family: sans-serif;
    }
    canvas {
      display: block;
      background: #111;
    }
    #ui {
      position: absolute;
      top: 10px;
      left: 10px;
      color: white;
      font-size: 18px;
      z-index: 1;
    }
    #finalScore {
      position: absolute;
      top: 45%;
      left: 50%;
      transform: translate(-50%, -50%);
      color: white;
      font-size: 26px;
      display: none;
      z-index: 2;
    }
    #restartBtn {
      position: absolute;
      top: 55%;
      left: 50%;
      transform: translate(-50%, -50%);
      padding: 20px 40px;
      font-size: 20px;
      background: #28a745;
      color: white;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      display: none;
      z-index: 2;
    }
  </style>
</head>
<body>
  <div id="ui">
    <div>Skor: <span id="score">0</span></div>
    <div>Can: <span id="lives">3</span></div>
  </div>
  <div id="finalScore"></div>
  <button id="restartBtn" onclick="restartGame()">Tekrar Başla</button>
  <canvas id="gameCanvas"></canvas>

  <script>
    const canvas = document.getElementById("gameCanvas");
    const ctx = canvas.getContext("2d");

    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    const uiScore = document.getElementById("score");
    const uiLives = document.getElementById("lives");
    const finalScoreText = document.getElementById("finalScore");
    const restartBtn = document.getElementById("restartBtn");

    let score = 0;
    let lives = 3;
    let gameOver = false;

    const player = {
      x: canvas.width / 2 - 20,
      y: canvas.height - 60,
      width: 40,
      height: 40,
      color: "blue",
      speed: 7
    };

    const bullets = [];
    const enemies = [];
    const enemyBullets = [];

    const keys = {};
    document.addEventListener("keydown", e => {
      keys[e.key] = true;
      if (e.key === " ") shoot();
    });
    document.addEventListener("keyup", e => keys[e.key] = false);

    function shoot() {
      bullets.push({
        x: player.x + player.width / 2 - 2,
        y: player.y,
        width: 4,
        height: 10,
        color: "yellow",
        speed: 10
      });
    }

    function createEnemy() {
      if (gameOver) return;
      enemies.push({
        x: Math.random() * (canvas.width - 40),
        y: -40,
        width: 40,
        height: 40,
        color: "red",
        speed: 2,
        fireCooldown: Math.random() * 100 + 50
      });
    }

    function update() {
      if (gameOver) return;

      if (keys["a"] || keys["A"]) player.x -= player.speed;
      if (keys["d"] || keys["D"]) player.x += player.speed;
      player.x = Math.max(0, Math.min(canvas.width - player.width, player.x));

      bullets.forEach((b, i) => {
        b.y -= b.speed;
        if (b.y < 0) bullets.splice(i, 1);
      });

      enemies.forEach((e, i) => {
        e.y += e.speed;
        e.fireCooldown--;
        if (e.fireCooldown <= 0) {
          enemyBullets.push({
            x: e.x + e.width / 2 - 2,
            y: e.y + e.height,
            width: 4,
            height: 10,
            color: "orange",
            speed: 4
          });
          e.fireCooldown = Math.random() * 100 + 50;
        }

        if (e.y > canvas.height) enemies.splice(i, 1);
      });

      enemyBullets.forEach((b, i) => {
        b.y += b.speed;
        if (b.y > canvas.height) enemyBullets.splice(i, 1);
      });

      enemies.forEach((e, ei) => {
        bullets.forEach((b, bi) => {
          if (b.x < e.x + e.width && b.x + b.width > e.x &&
              b.y < e.y + e.height && b.y + b.height > e.y) {
            enemies.splice(ei, 1);
            bullets.splice(bi, 1);
            score += 10;
            uiScore.textContent = score;
          }
        });
      });

      enemyBullets.forEach((b, i) => {
        if (b.x < player.x + player.width && b.x + b.width > player.x &&
            b.y < player.y + player.height && b.y + b.height > player.y) {
          enemyBullets.splice(i, 1);
          lives--;
          uiLives.textContent = lives;
          if (lives <= 0) endGame();
        }
      });
    }

    function draw() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      ctx.fillStyle = player.color;
      ctx.fillRect(player.x, player.y, player.width, player.height);

      bullets.forEach(b => {
        ctx.fillStyle = b.color;
        ctx.fillRect(b.x, b.y, b.width, b.height);
      });

      enemies.forEach(e => {
        ctx.fillStyle = e.color;
        ctx.fillRect(e.x, e.y, e.width, e.height);
      });

      enemyBullets.forEach(b => {
        ctx.fillStyle = b.color;
        ctx.fillRect(b.x, b.y, b.width, b.height);
      });
    }

    function gameLoop() {
      update();
      draw();
      requestAnimationFrame(gameLoop);
    }

    function endGame() {
      gameOver = true;
      finalScoreText.textContent = `Skorunuz: ${score}`;
      finalScoreText.style.display = "block";
      restartBtn.style.display = "block";
    }

    function restartGame() {
      score = 0;
      lives = 3;
      uiScore.textContent = score;
      uiLives.textContent = lives;
      player.x = canvas.width / 2 - 20;
      bullets.length = 0;
      enemies.length = 0;
      enemyBullets.length = 0;
      gameOver = false;
      finalScoreText.style.display = "none";
      restartBtn.style.display = "none";
    }

    setInterval(createEnemy, 1500);
    gameLoop();

    window.addEventListener("resize", () => {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
    });
  </script>
</body>
</html>
