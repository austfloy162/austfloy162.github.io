<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bouncing Box: Cyber</title>
    
    
    <script src="https://cdn.tailwindcss.com"></script>
    
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;700&display=swap');
        body { 
            font-family: 'Inter', sans-serif; 
            background-color: #1f2937; 
        }
        
        
        #gameBoard {
            background-color: #111827; 
            box-shadow: 0 10px 15px rgba(0, 0, 0, 0.5);
            border: 2px solid #0891b2; 
        }
        
        #gameBox {
            position: absolute;
            left: 0px;
            top: 50%;
            transform: translateY(-50%);
            
            /* Theme styling */
            background: linear-gradient(145deg, #fde047, #facc15); 
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2), 0 0 20px rgba(250, 204, 21, 0.5);
            transition: background 0.2s ease;
        }

        #gameBox:hover {
            background: linear-gradient(145deg, #fef08a, #fde047); 
        }

        .game-container {
            padding: 1rem;
            max-width: 1200px;
            margin: 0 auto;
        }

        @media (max-width: 768px) {
            .game-container {
                padding: 0.5rem;
            }
        }
    </style>
</head>
<body class="text-gray-200">
    <div class="game-container">
        
        <!-- Header -->
        <h1 class="text-3xl font-bold text-center mb-4 text-cyan-400 border-b border-cyan-700 pb-2">
            Bouncing Box
        </h1>
        
        <!-- Score Display -->
        <div id="game-info" class="text-center mb-4 p-3 bg-gray-800 rounded-lg shadow-md">
            <p class="text-yellow-400 text-xl">Score: <span id="score-display">0</span></p>
        </div>

        <div id="gameBoard" class="w-full h-64 rounded-lg overflow-hidden relative">
            <div id="gameBox" class="w-20 h-20 rounded-md cursor-pointer flex items-center justify-center text-3xl font-bold text-gray-900">
                0
            </div>
        </div>
        
    </div>

    <script>
        window.addEventListener('load', () => {
            'use strict';

            const box = document.getElementById("gameBox");
            const board = document.getElementById("gameBoard");
            const scoreDisplay = document.getElementById("score-display");

            if (!box || !board || !scoreDisplay) {
                console.error("Error: Could not find game elements (box, board, or scoreDisplay).");
                return;
            }

            const boardWidth = board.clientWidth;
            const boxWidth = box.clientWidth;

            let positionX = 0;
            let points = 0;
            
            let speed = 4;

            function moveBoxTo(newPositionX) {
                box.style.left = newPositionX + 'px';
            }

            function changeBoxText(text) {
                box.textContent = text;
                scoreDisplay.textContent = text;
            }

            function handleBoxClick() {
                positionX = 0;
                moveBoxTo(positionX);

                points++;

                speed = (1 * points) + 4;

                changeBoxText(points);
            }

            function gameLoop() {
                positionX += speed;

                if (positionX > (boardWidth - boxWidth)) {
                    positionX = boardWidth - boxWidth; 
                    speed *= -1; 
                } else if (positionX < 0) {
                    positionX = 0; 
                    speed *= -1; 
                }
                
                moveBoxTo(positionX);

                requestAnimationFrame(gameLoop);
            }

            changeBoxText(0);
            
            box.addEventListener("click", handleBoxClick);

            requestAnimationFrame(gameLoop);
        });
    </script>
</body>
</html>