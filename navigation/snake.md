---
layout: base 
title: snake 
search_exclude: false
permalink: /snake/
---
# Snake Game Implementation

Below is the code to create a simple Snake game using JavaScript and HTML. Save this code as an `.html` file and open it in your browser to play the game.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Game</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #333;
            color: white;
            font-family: Arial, sans-serif;
        }
        canvas {
            background-color: #000;
            display: block;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas" width="400" height="400"></canvas>
    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        const gridSize = 20;
        let snake = [{ x: gridSize * 5, y: gridSize * 5 }];
        let food = { x: gridSize * 10, y: gridSize * 10 };
        let dx = gridSize, dy = 0;
        let gameInterval;

        const drawRect = (x, y, color) => {
            ctx.fillStyle = color;
            ctx.fillRect(x, y, gridSize, gridSize);
        };

        const spawnFood = () => {
            food.x = Math.floor(Math.random() * canvas.width / gridSize) * gridSize;
            food.y = Math.floor(Math.random() * canvas.height / gridSize) * gridSize;
        };

        const updateGame = () => {
            const head = { x: snake[0].x + dx, y: snake[0].y + dy };
            snake.unshift(head);

            if (head.x === food.x && head.y === food.y) {
                spawnFood();
            } else {
                snake.pop();
            }

            if (head.x < 0 || head.y < 0 || head.x >= canvas.width || head.y >= canvas.height || snake.slice(1).some(segment => segment.x === head.x && segment.y === head.y)) {
                clearInterval(gameInterval);
                alert('Game Over!');
            }
        };

        const drawGame = () => {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            snake.forEach(segment => drawRect(segment.x, segment.y, 'lime'));
            drawRect(food.x, food.y, 'red');
        };

        const gameLoop = () => {
            updateGame();
            drawGame();
        };

        const changeDirection = (event) => {
            switch (event.key) {
                case 'ArrowUp': if (dy === 0) { dx = 0; dy = -gridSize; } break;
                case 'ArrowDown': if (dy === 0) { dx = 0; dy = gridSize; } break;
                case 'ArrowLeft': if (dx === 0) { dx = -gridSize; dy = 0; } break;
                case 'ArrowRight': if (dx === 0) { dx = gridSize; dy = 0; } break;
            }
        };

        document.addEventListener('keydown', changeDirection);
        gameInterval = setInterval(gameLoop, 100);
    </script>
</body>
</html>
