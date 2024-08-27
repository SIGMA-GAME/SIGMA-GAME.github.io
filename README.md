<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NOT a snake game</title>
    <style>
        body {
            display: flex;
            flex-direction: column;
            align-items: center;
            margin: 0;
            background-color: #f0f0f0;
        }
        canvas {
            background-color: #000;
        }
        #menu, #gameOver {
            display: none;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(0, 0, 0, 0.8);
            color: #fff;
            padding: 20px;
            border-radius: 8px;
            text-align: center;
        }
        #menu button, #gameOver button {
            display: block;
            margin-top: 10px;
        }
        #controls {
            margin: 20px;
        }
        label {
            margin-right: 10px;
        }
    </style>
</head>
<body>
    <div id="menu">
        <h1>Snake Game|kinda buggy|</h1>
        <h1> starting speed is 100 ms
	<h1> for speed lower=faster higher=slower
	<h1>
        <label for="speed">Speed (ms):</label>
        <input type="number" id="speed" value="100" min="10" max="1000">
        <label for="snakeColor">Snake Color:</label>
        <input type="color" id="snakeColor" value="#00FF00">
        <button id="startButton">Start Game</button>
    </div>
    <div id="gameOver">
        <h1>Game Over</h1>
        <div id="finalScore"></div>
        <button id="restartButton">Restart Game</button>
        <button id="backToMenuButton">Back to Menu</button>
    </div>
    <canvas id="gameCanvas" width="400" height="350"></canvas>
    <div id="score">Score: 0</div>

    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const speedInput = document.getElementById('speed');
        const colorInput = document.getElementById('snakeColor');
        const scoreDisplay = document.getElementById('score');
        const menu = document.getElementById('menu');
        const gameOverMenu = document.getElementById('gameOver');
        const startButton = document.getElementById('startButton');
        const restartButton = document.getElementById('restartButton');
        const backToMenuButton = document.getElementById('backToMenuButton');
        const finalScoreDisplay = document.getElementById('finalScore');

        const scale = 20;
        const rows = canvas.height / scale;
        const cols = canvas.width / scale;
        let snake, food, dx, dy, score, speed, gameInterval;

        function initGame() {
            snake = [{ x: 5 * scale, y: 5 * scale }];
            food = { x: Math.floor(Math.random() * cols) * scale, y: Math.floor(Math.random() * rows) * scale };
            dx = scale;
            dy = 0;
            score = 0;
            speed = parseInt(speedInput.value, 10);
            document.addEventListener('keydown', changeDirection);
            gameLoop();
            menu.style.display = 'none';
            gameOverMenu.style.display = 'none';
            canvas.style.display = 'block';
        }

        function gameLoop() {
            gameInterval = setInterval(() => {
                clearCanvas();
                drawFood();
                moveSnake();
                drawSnake();
                updateScore();
                checkCollision();
            }, speed);
        }

        function clearCanvas() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
        }

        function drawSnake() {
            const snakeColor = colorInput.value;
            ctx.fillStyle = snakeColor;
            snake.forEach(part => ctx.fillRect(part.x, part.y, scale, scale));
        }

        function drawFood() {
            ctx.fillStyle = 'red';
            ctx.fillRect(food.x, food.y, scale, scale);
        }

        function moveSnake() {
            const head = { x: snake[0].x + dx, y: snake[0].y + dy };
            snake.unshift(head);

            if (head.x === food.x && head.y === food.y) {
                score++;
                food = { x: Math.floor(Math.random() * cols) * scale, y: Math.floor(Math.random() * rows) * scale };
            } else {
                snake.pop();
            }
        }

        function updateScore() {
            scoreDisplay.textContent = `Score: ${score}`;
        }

        function changeDirection(event) {
            switch (event.key) {
                case 'ArrowUp':
                    if (dy === 0) { dx = 0; dy = -scale; }
                    break;
                case 'ArrowDown':
                    if (dy === 0) { dx = 0; dy = scale; }
                    break;
                case 'ArrowLeft':
                    if (dx === 0) { dx = -scale; dy = 0; }
                    break;
                case 'ArrowRight':
                    if (dx === 0) { dx = scale; dy = 0; }
                    break;
            }
        }

        function checkCollision() {
            const head = snake[0];
            if (
                head.x < 0 || head.x >= canvas.width ||
                head.y < 0 || head.y >= canvas.height ||
                snake.slice(1).some(part => part.x === head.x && part.y === head.y)
            ) {
                endGame();
            }
        }

        function endGame() {
            clearInterval(gameInterval);
            finalScoreDisplay.textContent = `Your score: ${score}`;
            canvas.style.display = 'none';
            gameOverMenu.style.display = 'block';
        }

        startButton.addEventListener('click', () => {
            menu.style.display = 'none';
            initGame();
        });

        restartButton.addEventListener('click', () => {
            gameOverMenu.style.display = 'none';
            initGame();
        });

        backToMenuButton.addEventListener('click', () => {
            canvas.style.display = 'none';
            gameOverMenu.style.display = 'none';
            menu.style.display = 'block';
        });

        // Show the menu at the start
        menu.style.display = 'block';
    </script>
</body>
</html>
