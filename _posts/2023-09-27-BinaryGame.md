<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Traffic Testing Game</title>
    <style>
        canvas {
            border: 1px solid #000;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas" width="600" height="400"></canvas>

    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");

        let carX = canvas.width / 2 - 25;
        let carY = canvas.height - 120;
        const CAR_WIDTH = 50;
        const CAR_HEIGHT = 100;
        const OBSTACLE_SPEED = 5;
        const obstacles = [];
        const keys = {};
        let gameRunning = true;

        document.addEventListener('keydown', function(event) {
            keys[event.key] = true;
        });

        document.addEventListener('keyup', function(event) {
            keys[event.key] = false;
        });

        function handleInput() {
            // Handle user input
            if (keys['ArrowLeft'] && carX > 0) {
                carX -= 5;
            }
            if (keys['ArrowRight'] && carX < canvas.width - CAR_WIDTH) {
                carX += 5;
            }
            if (keys['ArrowUp'] && carY > 0) {
                carY -= 5;
            }
            if (keys['ArrowDown'] && carY < canvas.height - CAR_HEIGHT) {
                carY += 5;
            }
        }

        function updateGame() {
            // Update game state
            if (!gameRunning) {
                return; // Stop updating if the game is not running
            }

            // Move obstacles
            for (let i = 0; i < obstacles.length; i++) {
                obstacles[i].y += OBSTACLE_SPEED;

                // Check for collision between car and obstacle
                if (
                    carX < obstacles[i].x + obstacles[i].width &&
                    carX + CAR_WIDTH > obstacles[i].x &&
                    carY < obstacles[i].y + obstacles[i].height &&
                    carY + CAR_HEIGHT > obstacles[i].y
                ) {
                    console.log("Collision Detected!");
                    gameRunning = false; // Stop the game on collision
                }
            }
        }

        function drawGame() {
            // Draw game elements on the canvas
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Draw car
            ctx.fillStyle = "rgb(0, 100, 200)";
            ctx.fillRect(carX, carY, CAR_WIDTH, CAR_HEIGHT);

            // Draw obstacles
            ctx.fillStyle = "black";
            for (const obstacle of obstacles) {
                ctx.fillRect(obstacle.x, obstacle.y, obstacle.width, obstacle.height);
            }
        }

        function gameLoop() {
            updateGame();
            drawGame();
            handleInput();
            requestAnimationFrame(gameLoop);
        }

        // Start the game loop
        gameLoop();

        // Example: You can add obstacle creation logic here if needed
        function createObstacle() {
            const obstacleWidth = Math.random() * (150 - 50) + 50;
            const obstacleX = Math.random() * (canvas.width - obstacleWidth);
            const obstacleY = -20;
            obstacles.push({ x: obstacleX, y: obstacleY, width: obstacleWidth, height: 20 });
        }

        setInterval(createObstacle, 1000);  // Create obstacles every second
    </script>
</body>
</html>
