<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Traffic Testing Game Boogollo</title>
    <style>
        canvas {
            background-color: red;
            border: 1px solid #000;
        }
    </style>
</head>
<body>
    <div style = "float:left">
        <canvas id="gameCanvas" width="600" height="400" style = "display:inline-block"></canvas>
        <div style = "min-width:300px; display:inline-block;" id = "sideMenu">
            <div id = "originalColor" style = "color:black;"></div>
            <div id = "invertedColor" style = "color:black;"></div>
            <input id = "redSlide" style = "width:256px" type = "range" max="255"/>
            <input id = "greenSlide" style = "width:256px" type = "range" max="255"/>
            <input id = "blueSlide" style = "width:256px" type = "range" max="255"/>
            <br>
            <div id = "objects" style = "padding= 0px; margin = 0px"></div>
            <button onclick = "js:startUp();">Start Game</button>
        </div>
    </div>
    <script>
        function convertColor(colorIndex) {
            // Get the input hex color value
            // Convert hex to binary
        // const binaryValue = hexToBinary(color);
            // Invert the binary
        // const invertedBinary = invertBinary(binaryValue);
            // Convert binary back to hex
            console.log("Current Color is called as a function")
            const displayedColors = `${colors[colorIndex].split(" ")[0]},${colors[colorIndex].split(" ")[1]},${colors[colorIndex].split(" ")[2]}`
            var invertedHex = ""
            for(x = 0; x < 2; x++){
                invertedHex += parseInt(255 - parseInt(colors[colorIndex].split(" ")[x])) + "," 
            };
            invertedHex += parseInt(255 - parseInt(colors[colorIndex].split(" ")[2]))
            // Display the result
            console.log("Change should be in", "")
            // colors[colorSelect] = `${colors[colorIndex].split(" ")[0]},${colors[colorIndex].split(" ")[1]},${colors[colorIndex].split(" ")[2]}`
            console.log("Colors are", colors, displayedColors)
            console.log("Inverted color is", invertedHex)
            displayColor('originalColor', 'Original Color', displayedColors, "notAvailable");
            displayColor('invertedColor', 'Inverted Color', invertedHex, "notAvailable");
        }
        function displayColor(elementId, label, colorToFill, binary) {
            // Add spaces every eight bits in the binary representation
            // const spacedBinary = binary.replace(/(.{8})/g, "$1 ");
            const element = document.getElementById(elementId);
            element.innerText = ""
            const displayProperText = document.createTextNode( `
                ${label}:
                Red: ${parseInt(colorToFill.split(",")[0]).toString(2)}
                Green: ${parseInt(colorToFill.split(",")[1]).toString(2)}
                Blue: ${parseInt(colorToFill.split(",")[2]).toString(2)}`)
                // ` <p>Binary: ${spacedBinary}</p>
            // `;
            console.log("Color is", colorToFill)
            // element.style.backgroundColor = colorToFill;
            // element.style.color = // getContrastColor(colorToFill); // Set text color for better visibility
            element.appendChild(displayProperText)
        }
        function hexToBinary(hex) {
            if (!/^[0-9A-Fa-f]+$/.test(hex)) {
                throw new Error("Invalid hex input");
            }
            let decimalValue = parseInt(hex, 16);
            let binaryValue = decimalValue.toString(2).padStart(24, '0'); // Ensure 24 bits
            return binaryValue;
        }
        function invertBinary(binaryString) {
            if (!/^[01]{24}$/.test(binaryString)) {
                throw new Error("Invalid binary input");
            }
            let invertedBinary = binaryString
                .split('')
                .map(bit => (bit === '0' ? '1' : '0'))
                .join('');
            return invertedBinary;
        }
        function binaryToHex(binaryString) {
            if (!/^[01]{24}$/.test(binaryString)) {
                throw new Error("Invalid binary input");
            }
            let decimalValue = parseInt(binaryString, 2);
            let hexValue = decimalValue.toString(16).toUpperCase().padStart(6, '0'); // Ensure 6 digits
            return '#' + hexValue;
        }
        function getContrastColor(hexColor) {
            // Function to determine text color based on background color
            const r = parseInt(hexColor.slice(1, 3), 16);
            const g = parseInt(hexColor.slice(3, 5), 16);
            const b = parseInt(hexColor.slice(5, 7), 16);
            const brightness = (r * 299 + g * 587 + b * 114) / 1000;
            return brightness > 128 ? 'black' : 'white';
        }
    </script>
    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");
        let carX = canvas.width / 2 - 25;
        let carY = canvas.height - 120;
        const CAR_WIDTH = 50;
        const CAR_HEIGHT = 100;
        var OBSTACLE_SPEED = 5;
        var colors = ["0 0 255", "0 0 0", "255 255 0", "0 127 0", "0 0 255"]
        var controlingColorValues = ["Player", "Obstacle", "Mystery", "Speed", "Shield"]
        var colorSelect = 0
        var currentElementPosition = 0
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
        document.addEventListener('input', function(event){
            console.log(colorSelect)
            switch(event.target.id){
                case "redSlide":
                    console.log("Red value is A", colors[colorSelect].split(" ")[0])
                    colors[colorSelect] = String(event.target.value) + " " + String(colors[colorSelect].split(" ")[1] + " " + String(colors[colorSelect].split(" ")[2]))
                    document.getElementById("orginalColor").innerText = document.createTextNode( `
                ${label}:
                Red: ${parseInt(colorToFill.split(",")[0]).toString(2)}
                Green: ${parseInt(colorToFill.split(",")[1]).toString(2)}
                Blue: ${parseInt(colorToFill.split(",")[2]).toString(2)}`)
                    break
                case "greenSlide":
                    console.log("Green value is A", colors[colorSelect].split(" ")[1])
                    colors[colorSelect] = String(colors[colorSelect].split(" ")[0]) + " " + String(event.target.value) + " " + String(colors[colorSelect].split(" ")[2])
                    break
                case "blueSlide":
                    console.log("Blue value is A", colors[colorSelect].split(" ")[2], "from", colors[colorSelect], "part of", colors[colorSelect].split(" "))
                    // colors[colorSelect].split(" ")[2] = event.target.value
                    colors[colorSelect] = String(colors[colorSelect].split(" ")[0] + " " + colors[colorSelect].split(" ")[1] + " " + event.target.value)
                    console.log("Final Output for blue is", colors[colorSelect], "color select value is,", colorSelect)
                    break
            }
            console.log("Changed Value From", event.target.id, "Is", event.target.value)
        })
        function showSliders(colorIndex){
            convertColor(colorIndex)
            fillNum = colors[colorIndex]
            colorSelect = colorIndex
            console.log("Total String", fillNum)
            document.getElementById("redSlide").value = fillNum.split(" ")[0]
            document.getElementById("greenSlide").value = fillNum.split(" ")[1]
            document.getElementById("blueSlide").value = fillNum.split(" ")[2]
        };
        function initiateSliders(){
            for(let fill in colors){
                let fillColor = colors[fill]
                console.log(fillColor)
                let displayFill = document.createElement("button")
                displayFill.innerHTML = String(controlingColorValues[fill])
                console.log(fillColor)
                console.log(colors)
                //
                document.getElementById("objects").appendChild(displayFill);
                console.log("Testing Slice", fillColor.slice(0, 8));
                displayFill.onclick = function(){showSliders(fill);};
            }
        }
        initiateSliders()
        convertColor(0)
        function handleInput() {
            // Handle user input
            if ((keys['ArrowLeft'] || keys['a']) && carX > 0){ 
                carX -= 5;
            } if((keys['ArrowRight'] || keys['d']) && carX < canvas.width - CAR_WIDTH){
                carX += 5;
            } if ((keys['ArrowUp'] || keys['w']) && carY > 0){
                carY -= 5;
            } if ((keys['ArrowDown'] || keys['s']) && carY < canvas.height - CAR_HEIGHT){
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
                mysteryBoxes[i].y += speedBoostActive ? OBSTACLE_SPEED * 0.5: OBSTACLE_SPEED;
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
                speedBoosts[i].y += speedBoostActive ? OBSTACLE_SPEED * 0.5: OBSTACLE_SPEED;
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
                shields[i].y += speedBoostActive ? OBSTACLE_SPEED * 0.5: OBSTACLE_SPEED
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
            ctx.fillStyle = `rgb(${colors[0].split(" ")[0]}, ${colors[0].split(" ")[1]}, ${colors[0].split(" ")[2]})`;
            ctx.fillRect(carX, carY, CAR_WIDTH, CAR_HEIGHT);
            // Draw obstacles
            ctx.fillStyle = `rgb(${colors[1].split(" ")[0]}, ${colors[1].split(" ")[1]}, ${colors[1].split(" ")[2]})`;
            for (const obstacle of obstacles) {
                ctx.fillRect(obstacle.x, obstacle.y, obstacle.width, obstacle.height);
            }
            // Draw mystery boxes
            ctx.fillStyle = `rgb(${colors[2].split(" ")[0]}, ${colors[2].split(" ")[1]}, ${colors[2].split(" ")[2]})`;
            for (const box of mysteryBoxes) {
                ctx.fillRect(box.x, box.y, box.width, box.height);
            }
            // Draw speed boosts
            ctx.fillStyle = `rgb(${colors[3].split(" ")[0]}, ${colors[3].split(" ")[1]}, ${colors[3].split(" ")[2]})`;
            for (const boost of speedBoosts) {
                ctx.fillRect(boost.x, boost.y, boost.width, boost.height);
            }
            // Draw shields
            ctx.fillStyle = `rgb(${colors[4].split(" ")[0]}, ${colors[4].split(" ")[1]}, ${colors[4].split(" ")[2]})`;
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
            setTimeout(createObstacle, speedBoostActive ? 2000: 1000)
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
        function startUp(){
            setTimeout(createObstacle, 1000)  // Create obstacles every second
            setInterval(createMysteryBox, speedBoostActive ? 11000: 5000); // Create mystery boxes every 5 seconds
            setInterval(createSpeedBoost, speedBoostActive ? 15400: 7000); // Create speed boosts every 7 seconds
            setInterval(createShield, speedBoostActive ? 220000: 10000); // Create shields every 10 seconds
            gameLoop()
        }
    </script>
</body>
</html>
