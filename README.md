<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Shooting Game</title>
    <style>
        body { margin: 0; overflow: hidden; }
        canvas { display: block; background: #f0f0f0; }
        #store, #instructions { position: absolute; top: 10px; left: 10px; background: white; padding: 10px; border: 1px solid black; }
        #instructions { top: 70px; }
    </style>
</head>
<body>
    <div id="store">
        Points: <span id="points">0</span><br>
        <button id="buyGun">Buy Advanced Gun (100 points)</button>
    </div>
    <div id="instructions">
        <h3>Instructions</h3>
        <ul>
            <li>Click on the mannequin to shoot and earn points.</li>
            <li>Earn 10 points for each hit.</li>
            <li>Accumulate points to buy advanced guns.</li>
            <li>Click the "Buy Advanced Gun" button to purchase when you have 100 points.</li>
        </ul>
    </div>
    <canvas id="gameCanvas"></canvas>
    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        let points = 0;
        let guns = ['Basic Gun'];
        let gunIndex = 0;

        const mannequin = {
            x: Math.random() * canvas.width,
            y: Math.random() * canvas.height,
            size: 50
        };

        const gun = {
            x: canvas.width / 2,
            y: canvas.height - 50,
            size: 20,
            color: 'blue'
        };

        document.getElementById('buyGun').addEventListener('click', () => {
            if (points >= 100) {
                points -= 100;
                guns.push('Advanced Gun');
                gunIndex = guns.length - 1;
            }
            updateStore();
        });

        function updateStore() {
            document.getElementById('points').textContent = points;
        }

        function drawMannequin() {
            ctx.fillStyle = 'red';
            ctx.fillRect(mannequin.x, mannequin.y, mannequin.size, mannequin.size);
        }

        function drawGun() {
            ctx.fillStyle = gun.color;
            ctx.beginPath();
            ctx.moveTo(gun.x, gun.y);
            ctx.lineTo(gun.x - gun.size, gun.y + gun.size);
            ctx.lineTo(gun.x + gun.size, gun.y + gun.size);
            ctx.closePath();
            ctx.fill();
        }

        function shoot(x, y) {
            if (x >= mannequin.x && x <= mannequin.x + mannequin.size &&
                y >= mannequin.y && y <= mannequin.y + mannequin.size) {
                points += 10;
                mannequin.x = Math.random() * canvas.width;
                mannequin.y = Math.random() * canvas.height;
                updateStore();
            }
        }

        canvas.addEventListener('click', (event) => {
            const rect = canvas.getBoundingClientRect();
            const x = event.clientX - rect.left;
            const y = event.clientY - rect.top;
            shoot(x, y);
        });

        function gameLoop() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawMannequin();
            drawGun();
            requestAnimationFrame(gameLoop);
        }

        gameLoop();
        updateStore();
    </script>
</body>
</html>

