<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Data Runner: The Byte Hunt</title>
    <!-- Load Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Use Inter font -->
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;700&display=swap');
        body { font-family: 'Inter', sans-serif; background-color: #1f2937; }
        canvas {
            display: block;
            background-color: #111827; /* Dark background */
            border-radius: 0.75rem;
            box-shadow: 0 10px 15px rgba(0, 0, 0, 0.5);
            margin: 0 auto;
        }
        .game-container {
            padding: 1rem;
            max-width: 1200px;
            margin: 0 auto;
        }
        .restart-button {
            background: linear-gradient(145deg, #1d4ed8, #2563eb);
            transition: all 0.2s ease;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
            border: none;
        }
        .restart-button:hover {
            background: linear-gradient(145deg, #2563eb, #3b82f6);
            transform: translateY(-1px);
            box-shadow: 0 6px 8px rgba(0, 0, 0, 0.3);
        }
        @media (max-width: 768px) {
            .game-container {
                padding: 0.5rem;
            }
        }
    </style>
</head>
<body>
    <div class="game-container">
        <h1 class="text-3xl font-bold text-center mb-4 text-cyan-400 border-b border-cyan-700 pb-2">Data Runner: The Byte Hunt</h1>
        <div id="game-info" class="text-center mb-4 p-3 bg-gray-800 rounded-lg shadow-md">
            <p class="text-yellow-400">Collect <span id="score-display">0</span> Data Shards & retrieve the central database!</p>
        </div>
        <canvas id="gameCanvas" class="w-full"></canvas>
        
        <!-- Game Over/Win Message Box -->
        <div id="message-box" class="fixed inset-0 bg-black bg-opacity-70 hidden items-center justify-center z-50">
            <div class="bg-gray-800 p-8 rounded-xl shadow-2xl text-center w-80 border-t-4 border-cyan-500 transform transition duration-300 scale-100">
                <p id="message-text" class="text-white text-xl font-bold mb-6"></p>
                <button onclick="restartGame()" class="restart-button text-white font-bold py-3 px-6 rounded-lg transition duration-200">
                    Restart Mission
                </button>
            </div>
        </div>
    </div>


    <!-- Firebase Imports for High Score (Kept separate for module import) -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";


        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : null;
        const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;


        let app, db, auth;
        let userId;


        async function initializeFirebase() {
            try {
                if (firebaseConfig) {
                    app = initializeApp(firebaseConfig);
                    db = getFirestore(app);
                    auth = getAuth(app);


                    if (initialAuthToken) {
                        await signInWithCustomToken(auth, initialAuthToken);
                    } else {
                        await signInAnonymously(auth);
                    }


                    userId = auth.currentUser?.uid || crypto.randomUUID();
                    console.log(`Firebase initialized. User ID: ${userId}`);
                } else {
                    console.error("Firebase config not found. Running in local mode.");
                }
            } catch (error) {
                console.error("Firebase initialization failed:", error);
            }
        }


        window.saveHighScore = async (score) => {
            if (!db || !userId) return;


            const path = `/artifacts/${appId}/users/${userId}/data_runner_scores/high_score`;
            try {
                await setDoc(doc(db, path), {
                    score: score,
                    timestamp: new Date().toISOString()
                }, { merge: true });
                console.log("High score saved successfully.");
            } catch (error) {
                console.error("Error saving high score:", error);
            }
        };


        initializeFirebase();
    </script>


    <!-- Game Logic -->
    <script>
        const GRAVITY = 0.98;
        const PLAYER_SPEED = 6;
        const JUMP_POWER = -16;
        const CAM_LERP = 0.1;


        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const scoreDisplay = document.getElementById('score-display');
        const messageBox = document.getElementById('message-box');
        const messageText = document.getElementById('message-text');


        let gameState = {
            player: null,
            platforms: [],
            collectables: [],
            cannons: [],
            bullets: [],
            score: 0,
            gameOver: false,
            gameWon: false,
            cameraX: 0,
            frame: 0 // Used for animation/rotation
        };


        let keys = {
            left: false,
            right: false,
            up: false,
            canJump: true
        };


        // --- ENTITY CLASSES ---


        class Player {
            constructor(x, y, w, h) {
                this.x = x;
                this.y = y;
                this.w = w;
                this.h = h;
                this.dx = 0;
                this.dy = 0;
                this.onGround = false;
            }


            update() {
                this.dy += GRAVITY;


                this.dx = 0;
                if (keys.left) this.dx = -PLAYER_SPEED;
                if (keys.right) this.dx = PLAYER_SPEED;


                this.x += this.dx;
                this.y += this.dy;


                this.checkPlatformCollision();


                if (this.y > canvas.height + 100) {
                    endGame("Mission Failed: Lost in the system void!", false);
                }
            }


            checkPlatformCollision() {
                this.onGround = false;
                for (const platform of gameState.platforms) {
                    // Check for collision when landing on top of a platform
                    if (this.x < platform.x + platform.w &&
                        this.x + this.w > platform.x &&
                        this.y + this.h > platform.y &&
                        this.y + this.h - this.dy <= platform.y) {

                        this.y = platform.y - this.h;
                        this.dy = 0;
                        this.onGround = true;
                        keys.canJump = true;
                        
                        // Transfer momentum from moving platform
                        if (platform.move) {
                             this.x += platform.dx;
                        }

                        return;
                    }
                }
            }


            jump() {
                if (this.onGround && keys.canJump) {
                    this.dy = JUMP_POWER;
                    keys.canJump = false;
                }
            }


            draw() {
                ctx.save();
                ctx.fillStyle = '#fde047'; // Cyber Yellow
                ctx.shadowColor = '#fde047';
                ctx.shadowBlur = 5;

                // Draw rounded rectangle for the player
                const rx = this.x - gameState.cameraX;
                const ry = this.y;
                const r = 5; // radius for corners

                ctx.beginPath();
                ctx.moveTo(rx + r, ry);
                ctx.lineTo(rx + this.w - r, ry);
                ctx.arcTo(rx + this.w, ry, rx + this.w, ry + r, r);
                ctx.lineTo(rx + this.w, ry + this.h - r);
                ctx.arcTo(rx + this.w, ry + this.h, rx + this.w - r, ry + this.h, r);
                ctx.lineTo(rx + r, ry + this.h);
                ctx.arcTo(rx, ry + this.h, rx, ry + this.h - r, r);
                ctx.lineTo(rx, ry + r);
                ctx.arcTo(rx, ry, rx + r, ry, r);
                ctx.closePath();
                ctx.fill();

                ctx.restore();
            }
        }


        class Platform {
            constructor(x, y, w, h, color, move = null) {
                this.x = x;
                this.y = y;
                this.w = w;
                this.h = h;
                this.color = color;
                this.dx = 0;
                this.move = move; // { startX, endX, speed }

                if (this.move) {
                    this.startX = move.startX;
                    this.endX = move.endX;
                    this.speed = move.speed;
                    this.dx = this.speed;
                }
            }

            update() {
                if (this.move) {
                    this.x += this.dx;
                    // Check boundaries and reverse direction
                    if (this.dx > 0 && this.x >= this.endX) {
                        this.dx *= -1;
                        this.x = this.endX;
                    } else if (this.dx < 0 && this.x <= this.startX) {
                        this.dx *= -1;
                        this.x = this.startX;
                    }
                }
            }

            draw() {
                ctx.fillStyle = this.color;
                ctx.fillRect(this.x - gameState.cameraX, this.y, this.w, this.h);
            }
        }


        class Collectable {
            constructor(x, y, type) {
                this.x = x;
                this.y = y;
                this.type = type;
                this.size = 20;
                this.collected = false;
            }
            
            draw() {
                if (this.collected) return;
                ctx.save();
                ctx.translate(this.x - gameState.cameraX + this.size / 2, this.y + this.size / 2);
                
                if (this.type === 'diamond') {
                    // Rotating Data Shard (Diamond)
                    const rotationSpeed = gameState.frame * 0.05;
                    ctx.rotate(rotationSpeed);

                    ctx.fillStyle = '#3b82f6'; // Light Blue
                    ctx.shadowColor = '#3b82f6';
                    ctx.shadowBlur = 8;
                    
                    // Simple Diamond Shape
                    const s = this.size * 0.7;
                    ctx.beginPath();
                    ctx.moveTo(0, -s / 2);
                    ctx.lineTo(s / 2, 0);
                    ctx.lineTo(0, s / 2);
                    ctx.lineTo(-s / 2, 0);
                    ctx.closePath();
                    ctx.fill();

                } else if (this.type === 'database') {
                    // Final Database Target (Red Cube)
                    ctx.fillStyle = '#ef4444';
                    ctx.shadowColor = '#ef4444';
                    ctx.shadowBlur = 15;
                    ctx.fillRect(-this.size / 2, -this.size / 2, this.size, this.size);
                    
                    ctx.fillStyle = '#ffffff';
                    ctx.shadowBlur = 0;
                    ctx.font = '10px Inter';
                    ctx.textAlign = 'center';
                    ctx.textBaseline = 'middle';
                    ctx.fillText('DB', 0, 0);
                }

                ctx.restore();
            }
        }


        class Cannon {
            constructor(x, y, direction, fireRate) {
                this.x = x;
                this.y = y;
                this.size = 20;
                this.direction = direction;
                this.fireRate = fireRate;
                this.timer = 0;
            }
            update() {
                this.timer++;
                if (this.timer >= this.fireRate) {
                    this.shoot();
                    this.timer = 0;
                }
            }
            shoot() {
                let bulletX = this.x + this.size / 2;
                let bulletY = this.y + this.size / 2;
                let dx = 0, dy = 0;

                switch (this.direction) {
                    case 'up': dy = -8; bulletY = this.y - 5; break;
                    case 'down': dy = 8; bulletY = this.y + this.size + 5; break;
                    case 'left': dx = -8; bulletX = this.x - 5; break;
                    case 'right': dx = 8; bulletX = this.x + this.size + 5; break;
                }
                gameState.bullets.push(new Bullet(bulletX, bulletY, dx, dy));
            }
            draw() {
                ctx.fillStyle = '#dc2626'; // Red Cannon Body
                ctx.fillRect(this.x - gameState.cameraX, this.y, this.size, this.size);

                ctx.fillStyle = '#fef08a'; // Yellow Barrel
                ctx.shadowColor = '#fef08a';
                ctx.shadowBlur = 5;

                // Draw barrel pointing in direction
                if (this.direction === 'up') ctx.fillRect(this.x + this.size/4 - gameState.cameraX, this.y - 10, this.size/2, 10);
                if (this.direction === 'down') ctx.fillRect(this.x + this.size/4 - gameState.cameraX, this.y + this.size, this.size/2, 10);
                if (this.direction === 'left') ctx.fillRect(this.x - 10 - gameState.cameraX, this.y + this.size/4, 10, this.size/2);
                if (this.direction === 'right') ctx.fillRect(this.x + this.size - gameState.cameraX, this.y + this.size/4, 10, this.size/2);

                ctx.shadowBlur = 0;
            }
        }


        class Bullet {
            constructor(x, y, dx, dy) {
                this.x = x;
                this.y = y;
                this.dx = dx;
                this.dy = dy;
                this.radius = 5;
                this.isAlive = true;
            }
            update() {
                this.x += this.dx;
                this.y += this.dy;
                // Check if outside reasonable bounds
                if (this.x < -1000 || this.x > 3500 || this.y < -1000 || this.y > canvas.height + 1000) {
                    this.isAlive = false;
                }
            }
            draw() {
                ctx.fillStyle = '#fef08a'; // Projectile color
                ctx.beginPath();
                ctx.arc(this.x - gameState.cameraX, this.y, this.radius, 0, Math.PI * 2);
                ctx.fill();
            }
        }


        // --- LEVEL DATA ---


        const levelData = {
            platforms: [
                // Ground 1 (Start Area)
                { x: 0, y: 580, w: 600, h: 20, color: '#374151' },
                
                // Jumps
                { x: 100, y: 500, w: 80, h: 15, color: '#10B981' },
                { x: 250, y: 450, w: 80, h: 15, color: '#10B981' },
                { x: 400, y: 400, w: 80, h: 15, color: '#10B981' },
                
                // Long platform with Cannon above
                { x: 650, y: 350, w: 300, h: 20, color: '#3B82F6' },
                
                // Bridge section with bottom cannon
                { x: 1050, y: 580, w: 400, h: 20, color: '#374151' },
                
                // Floating steps
                { x: 1500, y: 500, w: 50, h: 20, color: '#10B981' },
                { x: 1650, y: 450, w: 50, h: 20, color: '#10B981' },
                
                // ** MOVING PLATFORM ** (Moves between x=1800 and x=2200)
                { x: 1800, y: 350, w: 150, h: 20, color: '#FCD34D', move: { startX: 1800, endX: 2200, speed: 1.5 } },
                
                // Ground 2 (End Area)
                { x: 2350, y: 580, w: 300, h: 20, color: '#374151' }, 

                // Final Goal Platform
                { x: 2700, y: 500, w: 100, h: 20, color: '#3B82F6' },
            ],
            collectables: [
                { x: 300, y: 420, type: 'diamond' },
                { x: 450, y: 370, type: 'diamond' },
                { x: 800, y: 320, type: 'diamond' },
                { x: 1675, y: 420, type: 'diamond' },
                { x: 2000, y: 320, type: 'diamond' }, // On gap jump
                { x: 2740, y: 460, type: 'database' } // Final goal
            ],
            cannons: [
                { x: 750, y: 330, direction: 'down', fireRate: 150 },
                { x: 1400, y: 560, direction: 'left', fireRate: 120 },
                { x: 2400, y: 560, direction: 'right', fireRate: 90 }
            ],
        };


        // --- GAME CONTROL FUNCTIONS ---


        function initGame() {
            canvas.width = canvas.parentElement.clientWidth;
            canvas.height = Math.min(600, window.innerHeight * 0.8);
            window.addEventListener('resize', () => {
                canvas.width = canvas.parentElement.clientWidth;
                canvas.height = Math.min(600, window.innerHeight * 0.8);
            });


            // Initialize game state with level data
            gameState.platforms = levelData.platforms.map(p => new Platform(p.x, p.y, p.w, p.h, p.color, p.move));
            gameState.collectables = levelData.collectables.map(c => new Collectable(c.x, c.y, c.type));
            gameState.cannons = levelData.cannons.map(c => new Cannon(c.x, c.y, c.direction, c.fireRate));
            gameState.player = new Player(50, 500, 20, 20);
            gameState.bullets = [];
            gameState.score = 0;
            gameState.gameOver = false;
            gameState.gameWon = false;
            gameState.cameraX = 0;
            gameState.frame = 0;
            scoreDisplay.textContent = gameState.score;


            messageBox.classList.add('hidden', 'opacity-0');
            messageBox.classList.remove('flex');
        }


        function updateGame() {
            if (gameState.gameOver || gameState.gameWon) return;

            // 1. Update Entities
            gameState.player.update();
            gameState.platforms.forEach(p => p.update()); // Update moving platforms
            gameState.cannons.forEach(c => c.update());
            gameState.bullets.forEach(b => b.update());
            gameState.bullets = gameState.bullets.filter(b => b.isAlive);

            // 2. Check Collectables
            gameState.collectables.forEach(c => {
                const p = gameState.player;
                if (!c.collected &&
                    p.x < c.x + c.size &&
                    p.x + p.w > c.x &&
                    p.y < c.y + c.size &&
                    p.y + p.h > c.y) {

                    if (c.type === 'diamond') {
                        c.collected = true;
                        gameState.score += 1;
                        scoreDisplay.textContent = gameState.score;
                    } else if (c.type === 'database') {
                        endGame("SUCCESS! Central Database Retrieved. Mission Complete!", true);
                    }
                }
            });

            // 3. Check Bullet Collisions
            gameState.bullets.forEach(b => {
                const p = gameState.player;
                // Simple circle-square collision check (approximation)
                if (b.x + b.radius > p.x && b.x - b.radius < p.x + p.w && 
                    b.y + b.radius > p.y && b.y - b.radius < p.y + p.h) {
                    endGame("Mission Failed: Impact with hostile projectile!", false);
                }
            });

            // 4. Update Camera
            const targetX = gameState.player.x - canvas.width / 2 + gameState.player.w / 2;
            gameState.cameraX += (targetX - gameState.cameraX) * CAM_LERP;
            // Prevent camera from moving backwards past the start
            gameState.cameraX = Math.max(0, gameState.cameraX);
        }


        function drawGame() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Draw all game elements, adjusting for camera offset
            gameState.platforms.forEach(p => p.draw());
            gameState.cannons.forEach(c => c.draw());
            gameState.bullets.forEach(b => b.draw());
            gameState.collectables.forEach(c => c.draw());
            gameState.player.draw();
        }


        function gameLoop() {
            gameState.frame++;
            updateGame();
            drawGame();
            requestAnimationFrame(gameLoop);
        }


        function endGame(message, won = false) {
            if (gameState.gameOver || gameState.gameWon) return;


            if (won) {
                gameState.gameWon = true;
                messageText.innerHTML = `<span class="text-green-400">MISSION SUCCESS!</span><br>${message}<br>Final Score: ${gameState.score}`;
                if (typeof window.saveHighScore === 'function') {
                    window.saveHighScore(gameState.score);
                }
            } else {
                gameState.gameOver = true;
                messageText.innerHTML = `<span class="text-red-500">MISSION FAILED!</span><br>${message}<br>Score: ${gameState.score}`;
            }


            messageBox.classList.remove('hidden');
            messageBox.classList.add('flex', 'opacity-100');
        }


        window.restartGame = function() {
            initGame();
        }


        // --- INPUT HANDLING ---

        document.addEventListener('keydown', (e) => {
            if (gameState.gameOver || gameState.gameWon) return;
            switch(e.key) {
                case 'ArrowLeft':
                case 'a':
                    keys.left = true;
                    break;
                case 'ArrowRight':
                case 'd':
                    keys.right = true;
                    break;
                case ' ':
                case 'ArrowUp':
                case 'w':
                    // Only jump if it's the first keydown while jump is allowed
                    if (keys.canJump) {
                        gameState.player.jump();
                        keys.canJump = false; // Prevents spam jumping on keydown repeat
                    }
                    break;
            }
        });


        document.addEventListener('keyup', (e) => {
            switch(e.key) {
                case 'ArrowLeft':
                case 'a':
                    keys.left = false;
                    break;
                case 'ArrowRight':
                case 'd':
                    keys.right = false;
                    break;
                case ' ':
                case 'ArrowUp':
                case 'w':
                    keys.canJump = true; // Allow new jump on key up
                    break;
            }
        });


        // Initialize the game when the window loads
        window.onload = function() {
            initGame();
            gameLoop();
        };


    </script>
</body>
</html>