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
    <div style = "width:650; height:450; position: absolute; background-color: #005AFF;"></div>
    <canvas id="gameCanvas" width="600" height="400" style = "position: absolute; top: 110px; left: 25px"></canvas>
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
        let score = 0; // Initialize the score
        document.addEventListener('keydown', function(event) {
            keys[event.key] = true;
        });
        document.addEventListener('keyup', function(event) {
            keys[event.key] = false;
        });
        function handleInput() {
            // Handle user input
            keys.preventDefault;
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
                if(obstacles.length < 5){
                    obstacles[i].y += (obstacles.length + 1);
                }
                else{
                    obstacles[i].y += 5
                }
                // Check for collision between car and obstacle
                if (
                    carX < obstacles[i].x + obstacles[i].width &&
                    carX + CAR_WIDTH > obstacles[i].x &&
                    carY < obstacles[i].y + obstacles[i].height &&
                    carY + CAR_HEIGHT > obstacles[i].y
                ) {
                    console.log("Collision Detected!");
                    var endBackground = document.createElement("div")
                    endBackground.style.width = "100%"
                    endBackground.style.height = "100vh"
                    gameRunning = false; // Stop the game on collision
                }
                // Check if the car passes the obstacle
                if (obstacles[i].y > canvas.height) {
                    obstacles.splice(i, 1); // Remove the obstacle
                    score++; // Increment the score
                }
            }
        }
        function drawGame() {
            // Draw game elements on the canvas
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            // Draw car
            ctx.fillStyle = "#FFA500";
            ctx.fillRect(carX, carY, CAR_WIDTH, CAR_HEIGHT);
            // Draw obstacles
            ctx.fillStyle = "black";
            for (const obstacle of obstacles) {
                ctx.fillRect(obstacle.x, obstacle.y, obstacle.width, obstacle.height);
            }
            // Display the score
        }
        function gameLoop() {
            updateGame();
            drawGame();
            handleInput();
            ctx.fillStyle = "white"
            ctx.fillRect(0, 0, 150, 35)
            ctx.strokeRect(0, 0, 150, 35)
            ctx.fillStyle = "black";
            ctx.font = "20px Arial";
            ctx.fillText("Score: " + score, 10, 30);
            requestAnimationFrame(gameLoop);
        }
        // Start the game loop
        gameLoop();
        // Example: You can add obstacle creation logic here if needed
        function createObstacle() {
            var obstacleWidth = (obstacles.length + score) ** 2 + (Math.floor(Math.random() * 100) - 50)
            if(obstacleWidth > 200){
                obstacleWidth = 200
            }
            var obstacleX = carX - (obstacleWidth / 2) + (CAR_WIDTH / 2)
            if(obstacleX + obstacleWidth > canvas.width){
                console.log("Is called", obstacleX)
                obstacleX = canvas.width - obstacleWidth
            }
            const obstacleY = -20;
            obstacles.push({ x: obstacleX, y: obstacleY, width: obstacleWidth, height: 20 });
            setTimeout(createObstacle, 1000)
        } 
        createObstacle()
    </script>
</body>
</html>
