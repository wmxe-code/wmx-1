<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>星际战机 - HTML5 Canvas 飞机大战</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&display=swap');

        body {
            margin: 0;
            overflow: hidden;
            background-color: #0f172a;
            font-family: 'Orbitron', sans-serif;
            touch-action: none; /* Prevent pull-to-refresh on mobile */
        }

        #gameCanvas {
            display: block;
            width: 100%;
            height: 100%;
        }

        .ui-layer {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            padding: 20px;
            box-sizing: border-box;
        }

        .interactive-layer {
            pointer-events: auto;
        }

        .hud-text {
            text-shadow: 0 0 10px rgba(56, 189, 248, 0.8);
        }

        .btn-neon {
            background: rgba(15, 23, 42, 0.8);
            border: 1px solid #38bdf8;
            color: #38bdf8;
            box-shadow: 0 0 15px rgba(56, 189, 248, 0.3);
            transition: all 0.3s ease;
            backdrop-filter: blur(5px);
        }

        .btn-neon:hover {
            background: #38bdf8;
            color: #0f172a;
            box-shadow: 0 0 25px rgba(56, 189, 248, 0.6);
            transform: translateY(-2px);
        }

        .screen {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            background: rgba(15, 23, 42, 0.85);
            backdrop-filter: blur(8px);
            z-index: 10;
            transition: opacity 0.5s ease;
        }

        .hidden {
            opacity: 0;
            pointer-events: none;
            z-index: -1;
        }

        /* Scanline effect */
        .scanlines {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background: linear-gradient(to bottom, rgba(255,255,255,0), rgba(255,255,255,0) 50%, rgba(0,0,0,0.1) 50%, rgba(0,0,0,0.1));
            background-size: 100% 4px;
            pointer-events: none;
            z-index: 5;
            opacity: 0.3;
        }
    </style>
</head>
<body>

    <!-- Background Canvas -->
    <canvas id="gameCanvas"></canvas>
    
    <!-- Retro Scanlines Overlay -->
    <div class="scanlines"></div>

    <!-- HUD (Heads Up Display) -->
    <div id="hud" class="ui-layer hidden">
        <div class="flex justify-between items-start w-full max-w-4xl mx-auto">
            <div class="text-cyan-400">
                <div class="text-sm opacity-70">SCORE</div>
                <div id="scoreDisplay" class="text-3xl font-bold hud-text">0</div>
            </div>
            <div class="text-red-400 text-right">
                <div class="text-sm opacity-70">HP</div>
                <div class="flex gap-1" id="hpDisplay">
                    <i class="fas fa-heart"></i>
                    <i class="fas fa-heart"></i>
                    <i class="fas fa-heart"></i>
                </div>
            </div>
        </div>
        
        <!-- Mobile Controls Hint -->
        <div class="w-full text-center pb-8 opacity-50 text-white text-xs md:hidden">
            Drag to Move • Auto Fire
        </div>
    </div>

    <!-- Start Screen -->
    <div id="startScreen" class="screen interactive-layer">
        <h1 class="text-5xl md:text-7xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-cyan-400 to-blue-600 mb-4 tracking-wider" style="filter: drop-shadow(0 0 15px rgba(6,182,212,0.5));">
            STAR FIGHTER
        </h1>
        <p class="text-slate-400 mb-8 text-lg">Defend the galaxy from the alien invasion</p>
        
        <div class="space-y-4 w-full max-w-xs px-4">
            <button id="startBtn" class="btn-neon w-full py-3 rounded-lg font-bold text-lg uppercase tracking-widest">
                Start Mission
            </button>
            
            <div class="bg-slate-800/50 p-4 rounded-lg border border-slate-700 text-sm text-slate-300">
                <h3 class="text-cyan-400 font-bold mb-2"><i class="fas fa-gamepad mr-2"></i>Controls</h3>
                <ul class="space-y-1 text-xs">
                    <li><span class="text-white font-bold">Desktop:</span> Arrow Keys / WASD to Move, Space to Shoot</li>
                    <li><span class="text-white font-bold">Mobile:</span> Touch & Drag to Move (Auto-fire)</li>
                </ul>
            </div>
        </div>
    </div>

    <!-- Game Over Screen -->
    <div id="gameOverScreen" class="screen interactive-layer hidden">
        <h2 class="text-4xl md:text-6xl font-bold text-red-500 mb-2" style="text-shadow: 0 0 20px rgba(239, 68, 68, 0.6);">MISSION FAILED</h2>
        <div class="text-2xl text-white mb-8">Final Score: <span id="finalScore" class="text-cyan-400 font-bold">0</span></div>
        
        <div class="flex gap-4">
            <button id="restartBtn" class="btn-neon px-8 py-3 rounded-lg font-bold text-lg uppercase">
                Retry
            </button>
        </div>
    </div>

    <script src="game.js"></script>
</body>
</html>
