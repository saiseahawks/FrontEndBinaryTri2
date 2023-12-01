<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Traffic Testing Game</title>
    <style>
        canvas {
            background-color: red;
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
        var OBSTACLE_SPEED = 5;
        const POWERUP_SPEED_BOOST = 2;
        const POWERUP_SHIELD_DURATION = 5000; // 5 seconds
        const obstacles = [];
        const mysteryBoxes = [];
        const speedBoosts = [];
        const shields = [];
        const keys = {};
        let gameRunning = true;
        let score = 0; // Initialize the score
        let colorInverted = false; // Flag to track color inversion
        let speedBoostActive = false; // Flag to track speed boost power-up
        let shieldActive = false; // Flag to track shield power-up
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
                obstacles[i].y += speedBoostActive ? OBSTACLE_SPEED * 0.5: OBSTACLE_SPEED;
                // Check for collision between car and obstacle
                if (
                    carX < obstacles[i].x + obstacles[i].width &&
                    carX + CAR_WIDTH > obstacles[i].x &&
                    carY < obstacles[i].y + obstacles[i].height &&
                    carY + CAR_HEIGHT > obstacles[i].y &&
                    shieldActive == false
                ) {
                    console.log("Collision Detected!");
                    gameRunning = false
                    handleCollision();
                    return
                }
                else if(
                    carX < obstacles[i].x + obstacles[i].width &&
                    carX + CAR_WIDTH > obstacles[i].x &&
                    carY < obstacles[i].y + obstacles[i].height &&
                    carY + CAR_HEIGHT > obstacles[i].y &&
                    shieldActive == true
                ){
                    shieldActive == false
                }
                // Check if the car passes the obstacle
                if (obstacles[i].y > canvas.height) {
                    obstacles.splice(i, 1); // Remove the obstacle
                    score++; // Increment the score
                }
            }
            // Move mystery boxes
            for (let i = 0; i < mysteryBoxes.length; i++) {
                mysteryBoxes[i].y += OBSTACLE_SPEED;
                // Check for collision between car and mystery box
                if (
                    carX < mysteryBoxes[i].x + mysteryBoxes[i].width &&
                    carX + CAR_WIDTH > mysteryBoxes[i].x &&
                    carY < mysteryBoxes[i].y + mysteryBoxes[i].height &&
                    carY + CAR_HEIGHT > mysteryBoxes[i].y
                ) {
                    console.log("Mystery Box Hit!");
                    handleMysteryBox();
                    mysteryBoxes.splice(i, 1); // Remove the mystery box
                }
            }
            // Move speed boosts
            for (let i = 0; i < speedBoosts.length; i++) {
                speedBoosts[i].y += OBSTACLE_SPEED;
                // Check for collision between car and speed boost
                if (
                    carX < speedBoosts[i].x + speedBoosts[i].width &&
                    carX + CAR_WIDTH > speedBoosts[i].x &&
                    carY < speedBoosts[i].y + speedBoosts[i].height &&
                    carY + CAR_HEIGHT > speedBoosts[i].y
                ) {
                    console.log("Speed Boost Hit!");
                    handleSpeedBoost();
                    speedBoosts.splice(i, 1); // Remove the speed boost
                }
            }
            // Move shields
            for (let i = 0; i < shields.length; i++) {
                shields[i].y += OBSTACLE_SPEED
                // Check for collision between car and shield
                if (
                    carX < shields[i].x + shields[i].width &&
                    carX + CAR_WIDTH > shields[i].x &&
                    carY < shields[i].y + shields[i].height &&
                    carY + CAR_HEIGHT > shields[i].y
                ) {
                    console.log("Shield Hit!");
                    handleShield();
                    shields.splice(i, 1); // Remove the shield
                }
            }
        }
        function handleCollision() {
            if (shieldActive) {
                console.log("Shield Protected!");
                shieldActive = false; // Deactivate the shield
            } else {
                gameRunning = false; // Stop the game on collision
                colorInverted = true; // Trigger color inversion
                ctx.fillStyle = "black";
                console.log("Is, called")
                ctx.globalAlpha = 0.3
                ctx.fillRect(0, 0, canvas.width, canvas.height);
                ctx.globalAlpha = 1
                ctx.fillStyle = "darkGreen";
                ctx.fillRect(100, 100, canvas.width - 200, canvas.height - 200);
                ctx.fillStyle = "white"
                ctx.textAlign = "center"
                ctx.fillText("Game Over", (canvas.width / 2), canvas.height / 2- 25)
                ctx.fillText("Click Screen to Try Again", (canvas.width / 2), canvas.height / 2 + 25)
                canvas.onclick = function(){window.location.reload()}
                canvas.style.filter = "invert(100%)";
            }
        }
        function handleMysteryBox() {
            // Implement cool effects for mystery box
            // For example, you can add additional points or power-ups here
            console.log("Mystery Box Effect!");
            score += 5; // Add 5 points to the score
        }
        function handleSpeedBoost() {
            console.log("Speed Boost Activated!");
            speedBoostActive = true; // Activate speed boost
            setTimeout(() => {
                speedBoostActive = false; // Deactivate speed boost after 5 seconds
            }, POWERUP_SHIELD_DURATION);
        }
        function handleShield() {
            console.log("Shield Activated!");
            shieldActive = true; // Activate shield
            setTimeout(() => {
                shieldActive = false; // Deactivate shield after 5 seconds
            }, POWERUP_SHIELD_DURATION);
        }
        function drawGame() {
            // Draw game elements on the canvas
            // Apply color inversion if needed
            if (colorInverted) {
                // ctx.fillStyle = "black";
                // ctx.fillRect(0, 0, canvas.width, canvas.height);
            }
            if(!gameRunning){
                return
            }
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            // Draw car
            ctx.fillStyle = "rgb(0, 100, 200)";
            ctx.fillRect(carX, carY, CAR_WIDTH, CAR_HEIGHT);
            // Draw obstacles
            ctx.fillStyle = "black";
            for (const obstacle of obstacles) {
                ctx.fillRect(obstacle.x, obstacle.y, obstacle.width, obstacle.height);
            }
            // Draw mystery boxes
            ctx.fillStyle = "yellow";
            for (const box of mysteryBoxes) {
                ctx.fillRect(box.x, box.y, box.width, box.height);
            }
            // Draw speed boosts
            ctx.fillStyle = "green";
            for (const boost of speedBoosts) {
                ctx.fillRect(boost.x, boost.y, boost.width, boost.height);
            }
            // Draw shields
            ctx.fillStyle = "blue";
            for (const shield of shields) {
                ctx.fillRect(shield.x, shield.y, shield.width, shield.height);
            }
            // Display the score
            ctx.fillStyle = "white"
            ctx.globalAlpha = 0.5
            ctx.fillRect(0, 0, 250, 100)
            ctx.globalAlpha = 1;
            ctx.fillStyle = "black";
            ctx.font = "20px Arial";
            ctx.fillText("Score: " + score, 10, 30);
            // Display power-up status
            ctx.fillText("Speed Boost: " + (speedBoostActive ? "Active" : "Inactive"), 10, 60);
            ctx.fillText("Shield: " + (shieldActive ? "Active" : "Inactive"), 10, 90);
            // Reset color inversion
            ctx.filter = "none";
        }
        function gameLoop() {
            updateGame();
            drawGame();
            handleInput();
            requestAnimationFrame(gameLoop);
        }
        // Start the game loop
        gameLoop();
        // Example: You can add obstacle, mystery box, speed boost, and shield creation logic here if needed
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
            if(obstacleWidth < 10){
                obstacleWidth = 10
            }
            const obstacleY = -20;
            obstacles.push({ x: obstacleX, y: obstacleY, width: obstacleWidth, height: 20 });
        }
        function createMysteryBox() {
            const boxWidth = 30;
            const boxX = Math.random() * (canvas.width - boxWidth);
            const boxY = -20;
            mysteryBoxes.push({ x: boxX, y: boxY, width: boxWidth, height: boxWidth });
        }
        function createSpeedBoost() {
            const boostWidth = 30;
            const boostX = Math.random() * (canvas.width - boostWidth);
            const boostY = -20;
            speedBoosts.push({ x: boostX, y: boostY, width: boostWidth, height: boostWidth });
        }
        function createShield() {
            const shieldWidth = 30;
            const shieldX = Math.random() * (canvas.width - shieldWidth);
            const shieldY = -20;
            shields.push({ x: shieldX, y: shieldY, width: shieldWidth, height: shieldWidth });
        }
        setInterval(createObstacle, speedBoostActive ? 1000:1200);  // Create obstacles every second
        setInterval(createMysteryBox, 5000); // Create mystery boxes every 5 seconds
        setInterval(createSpeedBoost, 7000); // Create speed boosts every 7 seconds
        setInterval(createShield, 10000); // Create shields every 10 seconds
    </script>
</body>
</html>