<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mini Games Page</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #f4f4f4;
            color: #333;
            margin: 0;
            padding: 0;
        }
        h1 {
            background-color: #4CAF50;
            color: white;
            padding: 20px;
            margin: 0;
        }
        .game {
            margin: 20px auto;
            padding: 20px;
            background: white;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            max-width: 400px;
        }
        canvas {
            display: block;
            margin: 0 auto;
            background-color: #eee;
            border: 1px solid #ccc;
        }
        button {
            padding: 10px 20px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        button:hover {
            background-color: #45a049;
        }
    </style>
</head>
<body>
    <h1>Mini Games Page</h1>

    <div class="game">
        <h2>Click Counter</h2>
        <p>Clicks: <span id="clickCount">0</span></p>
        <button onclick="incrementCounter()">Click Me!</button>
    </div>

    <div class="game">
        <h2>Guess the Number</h2>
        <p id="guessMessage">I am thinking of a number between 1 and 10.</p>
        <input type="number" id="guessInput" min="1" max="10">
        <button onclick="checkGuess()">Guess</button>
    </div>

    <div class="game">
        <h2>Dice Roller</h2>
        <p>Roll Result: <span id="diceResult">-</span></p>
        <button onclick="rollDice()">Roll the Dice</button>
    </div>

    <div class="game">
        <h2>Car Racing Game</h2>
        <canvas id="racingCanvas" width="400" height="300"></canvas>
        <button onclick="startRace()">Start Race</button>
    </div>

    <div class="game">
        <h2>Shooting Game</h2>
        <canvas id="shootingCanvas" width="400" height="300"></canvas>
        <p>Score: <span id="score">0</span></p>
        <button onclick="startShootingGame()">Start Shooting Game</button>
    </div>

    <script>
        // Click Counter Game
        let clickCount = 0;
        function incrementCounter() {
            clickCount++;
            document.getElementById('clickCount').textContent = clickCount;
        }

        // Guess the Number Game
        const secretNumber = Math.floor(Math.random() * 10) + 1;
        function checkGuess() {
            const userGuess = parseInt(document.getElementById('guessInput').value);
            const message = userGuess === secretNumber
                ? '🎉 Correct! You guessed the number!'
                : userGuess < secretNumber
                ? '📉 Too low! Try again.'
                : '📈 Too high! Try again.';
            document.getElementById('guessMessage').textContent = message;
        }

        // Dice Roller Game
        function rollDice() {
            const diceRoll = Math.floor(Math.random() * 6) + 1;
            document.getElementById('diceResult').textContent = diceRoll;
        }

        // Car Racing Game
        const canvas = document.getElementById('racingCanvas');
        const ctx = canvas.getContext('2d');
        let carX = 50;
        let carY = 250;
        let raceInterval;

        function drawTrack() {
            ctx.fillStyle = '#666';
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            ctx.fillStyle = '#fff';
            for (let i = 0; i < canvas.height; i += 40) {
                ctx.fillRect(canvas.width / 2 - 5, i, 10, 20);
            }
        }

        function drawCar() {
            ctx.fillStyle = 'red';
            ctx.fillRect(carX, carY, 30, 15);
        }

        function startRace() {
            clearInterval(raceInterval);
            carX = 50;
            raceInterval = setInterval(() => {
                ctx.clearRect(0, 0, canvas.width, canvas.height);
                drawTrack();
                drawCar();
                carX += 5;
                if (carX > canvas.width) {
                    clearInterval(raceInterval);
                    alert('Race Finished!');
                }
            }, 100);
        }

        drawTrack();
        drawCar();

        // Shooting Game
        const shootingCanvas = document.getElementById('shootingCanvas');
        const shootingCtx = shootingCanvas.getContext('2d');
        let targets = [];
        let score = 0;
        let shootingInterval;

        function createTarget() {
            return {
                x: Math.random() * (shootingCanvas.width - 30),
                y: Math.random() * (shootingCanvas.height - 30),
                size: 30,
            };
        }

        function drawTargets() {
            targets.forEach(target => {
                shootingCtx.fillStyle = 'blue';
                shootingCtx.fillRect(target.x, target.y, target.size, target.size);
            });
        }

        function startShootingGame() {
            clearInterval(shootingInterval);
            targets = [createTarget(), createTarget(), createTarget()];
            score = 0;
            document.getElementById('score').textContent = score;

            shootingInterval = setInterval(() => {
                shootingCtx.clearRect(0, 0, shootingCanvas.width, shootingCanvas.height);
                drawTargets();
            }, 50);

            shootingCanvas.onclick = (e) => {
                const rect = shootingCanvas.getBoundingClientRect();
                const clickX = e.clientX - rect.left;
                const clickY = e.clientY - rect.top;

                targets = targets.filter(target => {
                    const hit = clickX >= target.x && clickX <= target.x + target.size &&
                                clickY >= target.y && clickY <= target.y + target.size;
                    if (hit) {
                        score++;
                        document.getElementById('score').textContent = score;
                    }
                    return !hit;
                });

                if (targets.length === 0) {
                    clearInterval(shootingInterval);
                    alert('You win! Final score: ' + score);
                }
            };
        }
    </script>
</body>
</html>
