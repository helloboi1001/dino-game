# dino-game<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Dino Game</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="game-container">
        <div class="dino" id="dino"></div>
        <div class="obstacles-container" id="obstacles"></div>
        <div class="clouds-container" id="clouds"></div>
        <div class="ground-container"></div>
        <div class="score-container">
            <div>Score: <span id="score">0</span></div>
            <div>High Score: <span id="highScore">0</span></div>
        </div>
        <div class="game-over" id="gameOver">
            GAME OVER
            <button id="restartBtn" class="game-btn">Restart</button>
        </div>
        <div class="start-screen" id="startScreen">
            <button id="startBtn" class="game-btn">Start Game</button>
            <div class="controls">
                <p>Jump: Space/Up Arrow/Tap</p>
                <p>Duck: Down Arrow</p>
            </div>
        </div>
        <div class="mobile-controls">
            <button class="jump-btn" id="mobileJump">Jump</button>
            <button class="duck-btn" id="mobileDuck">Duck</button>
        </div>
    </div>
    
    <!-- Preload images -->
    <div style="display:none;">
        <img id="cactus1" src="https://raw.githubusercontent.com/wayou/t-rex-runner/master/assets/images/cactus-1.png">
        <img id="cactus2" src="https://raw.githubusercontent.com/wayou/t-rex-runner/master/assets/images/cactus-2.png">
        <img id="cactus3" src="https://raw.githubusercontent.com/wayou/t-rex-runner/master/assets/images/cactus-3.png">
        <img id="bird" src="https://raw.githubusercontent.com/wayou/t-rex-runner/master/assets/images/bird.png">
    </div>
    
    <script src="script.js"></script>
</body>
</html>/* Previous CSS remains the same until the obstacle classes */

.obstacle {
    position: absolute;
    bottom: 0;
    z-index: 2;
    image-rendering: pixelated;
    background-repeat: no-repeat;
    background-size: contain;
}

.cactus-small {
    width: 34px;
    height: 70px;
    background-image: url('https://raw.githubusercontent.com/wayou/t-rex-runner/master/assets/images/cactus-1.png');
}

.cactus-large {
    width: 50px;
    height: 100px;
    background-image: url('https://raw.githubusercontent.com/wayou/t-rex-runner/master/assets/images/cactus-2.png');
}

.cactus-double {
    width: 68px;
    height: 70px;
    background-image: url('https://raw.githubusercontent.com/wayou/t-rex-runner/master/assets/images/cactus-3.png');
}

.bird {
    width: 92px;
    height: 80px;
    background-image: url('https://raw.githubusercontent.com/wayou/t-rex-runner/master/assets/images/bird.png');
    animation: birdFly 0.8s steps(2) infinite;
}

@keyframes birdFly {
    0% { background-position: 0 0; }
    100% { background-position: -184px 0; }
}

/* Rest of the CSS remains the same */document.addEventListener('DOMContentLoaded', () => {
    // Previous code remains the same until obstacleTypes
    
    // Obstacle types with different sizes and images
    const obstacleTypes = [
        { 
            className: 'cactus-small', 
            width: 34, 
            height: 70,
            minGap: 120,
            speedOffset: 0
        },
        { 
            className: 'cactus-large', 
            width: 50, 
            height: 100,
            minGap: 150,
            speedOffset: 0
        },
        { 
            className: 'cactus-double', 
            width: 68, 
            height: 70,
            minGap: 180,
            speedOffset: 0
        },
        { 
            className: 'bird', 
            width: 92, 
            height: 80,
            minGap: 200,
            speedOffset: 0.5,
            isFlying: true
        }
    ];
    
    // Previous code remains the same until createObstacle
    
    function createObstacle() {
        if (isGameOver) return;
        
        const obstacleType = obstacleTypes[Math.floor(Math.random() * obstacleTypes.length)];
        const obstacle = document.createElement('div');
        obstacle.className = `obstacle ${obstacleType.className} move`;
        obstacle.style.left = '100%';
        obstacle.style.width = `${obstacleType.width}px`;
        obstacle.style.height = `${obstacleType.height}px`;
        
        if (obstacleType.isFlying) {
            obstacle.style.bottom = `${Math.random() > 0.5 ? 60 : 100}px`;
        } else {
            obstacle.style.bottom = '0';
        }
        
        obstacle.style.animationDuration = `${gameSpeed - obstacleType.speedOffset}s`;
        obstaclesContainer.appendChild(obstacle);
        
        // Remove obstacle when it's off screen
        setTimeout(() => {
            obstacle.remove();
        }, (gameSpeed - obstacleType.speedOffset) * 1000);
        
        // Schedule next obstacle with appropriate gap
        const minGap = Math.max(obstacleType.minGap, 600 - score * 0.5);
        const maxGap = minGap + 200;
        const randomGap = Math.random() * (maxGap - minGap) + minGap;
        
        obstacleTimeout = setTimeout(() => {
            createObstacle();
        }, randomGap);
    }
    
    // Rest of the code remains the same
});
