<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes, viewport-fit=cover">
    <title>🎮 Игровой портал | 4 игры в браузере</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,400;14..32,600;14..32,800&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(135deg, #0a0f1e 0%, #030812 100%);
            color: #eef4ff;
            line-height: 1.4;
        }

        .container {
            max-width: 1300px;
            margin: 0 auto;
            padding: 0 20px;
        }

        /* шапка */
        header {
            padding: 20px 0;
            text-align: center;
            border-bottom: 1px solid rgba(100,150,200,0.2);
            backdrop-filter: blur(10px);
            position: sticky;
            top: 0;
            background: rgba(10, 20, 35, 0.7);
            z-index: 100;
        }

        .logo {
            font-size: 2rem;
            font-weight: 800;
            background: linear-gradient(135deg, #aaffff, #ffaa88);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            letter-spacing: -0.5px;
        }

        .sub {
            font-size: 0.8rem;
            opacity: 0.7;
            margin-top: 5px;
        }

        /* сетка игр */
        .games-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(380px, 1fr));
            gap: 30px;
            padding: 40px 0;
        }

        .game-card {
            background: rgba(15, 25, 45, 0.6);
            backdrop-filter: blur(8px);
            border-radius: 36px;
            border: 1px solid rgba(100, 150, 220, 0.3);
            overflow: hidden;
            transition: transform 0.2s, border-color 0.2s;
        }

        .game-card:hover {
            transform: translateY(-5px);
            border-color: #6a9eff;
            background: rgba(25, 40, 65, 0.7);
        }

        .game-header {
            padding: 18px 20px 8px 20px;
            border-bottom: 1px solid #2a4a6a;
        }

        .game-header h2 {
            font-size: 1.5rem;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .game-desc {
            padding: 0 20px 12px 20px;
            font-size: 0.8rem;
            color: #bbd4ff;
        }

        .game-canvas-container {
            display: flex;
            justify-content: center;
            background: #050a12;
            padding: 15px 10px;
            border-bottom: 1px solid #1a3a55;
        }

        canvas {
            display: block;
            border-radius: 20px;
            box-shadow: 0 0 0 2px #2a5068, 0 8px 20px rgba(0,0,0,0.4);
            background: #0a1220;
            width: 100%;
            height: auto;
            cursor: pointer;
        }

        .game-controls {
            padding: 15px;
            display: flex;
            justify-content: center;
            gap: 15px;
            flex-wrap: wrap;
            border-top: 1px solid #1a3a55;
            background: rgba(0,0,0,0.3);
        }

        .ctrl-btn {
            background: #1e2f3f;
            border: none;
            font-size: 1rem;
            font-weight: bold;
            padding: 8px 20px;
            border-radius: 50px;
            color: #ddf4ff;
            box-shadow: 0 3px 0 #0a1a2a;
            cursor: pointer;
            transition: 0.02s linear;
            font-family: monospace;
            touch-action: manipulation;
        }
        .ctrl-btn:active {
            transform: translateY(2px);
            box-shadow: 0 1px 0 #0a1a2a;
        }
        .shoot-btn {
            background: #9b2f2f;
            box-shadow: 0 3px 0 #4a1515;
            color: #ffccaa;
        }
        .reset-game-btn {
            background: #2a4c6e;
            font-size: 0.8rem;
            padding: 6px 14px;
        }

        .game-stats {
            display: flex;
            justify-content: space-between;
            padding: 8px 20px;
            background: #03080f;
            font-size: 0.75rem;
            font-family: monospace;
            color: #ffdd99;
            border-top: 1px solid #2a4a6a;
        }

        footer {
            text-align: center;
            padding: 25px;
            border-top: 1px solid #2a4a6a;
            font-size: 0.75rem;
            color: #7f9fbf;
        }

        @media (max-width: 850px) {
            .games-grid {
                grid-template-columns: 1fr;
                gap: 30px;
            }
            .ctrl-btn { padding: 6px 14px; font-size: 0.85rem; }
        }
    </style>
</head>
<body>
<header>
    <div class="logo">🎮 GAME HUB</div>
    <div class="sub">4 игры в одном месте | Играй прямо в браузере</div>
</header>

<div class="container">
    <div class="games-grid" id="gamesGrid"></div>
</div>

<footer>
    🕹️ Все игры созданы специально для портала | Управление кнопками на экране
</footer>

<script>
    // --------------------------------------------------------------
    // 1. ИГРА: 2D ШУТЕР (зомби)
    // --------------------------------------------------------------
    function createShooterGame(containerId) {
        const container = document.getElementById(containerId);
        if (!container) return;
        
        container.innerHTML = `
            <div class="game-header"><h2>🔫 2D ШУТЕР</h2></div>
            <div class="game-desc">Уничтожай зомби, уворачивайся, набирай очки. Разные типы врагов!</div>
            <div class="game-canvas-container"><canvas id="shooterCanvas" width="400" height="400"></canvas></div>
            <div class="game-stats"><span>💀 ОЧКИ: <span id="shooterScore">0</span></span><span>❤️ ЗДОРОВЬЕ: <span id="shooterHealth">5</span></span><span>🔥 УБИЙСТВА: <span id="shooterKills">0</span></span></div>
            <div class="game-controls">
                <button class="ctrl-btn" id="shooterLeft">◀ ЛЕВО</button>
                <button class="ctrl-btn shoot-btn" id="shooterShoot">🔫 ОГОНЬ</button>
                <button class="ctrl-btn" id="shooterRight">ПРАВО ▶</button>
                <button class="ctrl-btn reset-game-btn" id="shooterReset">🔄 РЕСТАРТ</button>
            </div>
        `;
        
        const canvas = document.getElementById('shooterCanvas');
        const ctx = canvas.getContext('2d');
        const W = 400, H = 400;
        canvas.width = W; canvas.height = H;
        
        let playerX = W/2 - 32/2;
        const playerY = H - 55;
        let playerHealth = 5;
        let invincible = 0;
        let bullets = [];
        let enemies = [];
        let score = 0, kills = 0, gameOver = false;
        let fireDelay = 0, frame = 0;
        let leftPress = false, rightPress = false;
        
        function updateUI() {
            document.getElementById('shooterScore').innerText = score;
            document.getElementById('shooterHealth').innerText = playerHealth;
            document.getElementById('shooterKills').innerText = kills;
            if(playerHealth <= 0) gameOver = true;
        }
        
        function spawnEnemy() {
            let r = Math.random();
            let hp = 1, color = "#6a2e2e", size = 30, speed = 1.5 + Math.random();
            if(score > 30 && r<0.25) { hp=2; color="#4a2e6a"; size=32; speed*=0.9; }
            else if(score > 50 && r<0.3) { hp=1; color="#8a5a2a"; size=28; speed*=1.6; }
            enemies.push({ x: Math.random()*(W-size), y: -size, w:size, h:size, hp, color, speedY: speed });
        }
        
        function shoot() {
            if(gameOver || fireDelay>0) return;
            bullets.push({ x: playerX+16-2.5, y: playerY-8, w:5, h:9, damage:1 });
            fireDelay = 12;
        }
        
        function updateGame() {
            if(gameOver) return;
            if(fireDelay>0) fireDelay--;
            if(frame % Math.max(28, 45-Math.floor(score/35)) === 0 && enemies.length<10) spawnEnemy();
            
            for(let i=0;i<enemies.length;i++) {
                let e=enemies[i]; e.y+=e.speedY;
                if(invincible<=0 && !gameOver && e.x<playerX+32 && e.x+e.w>playerX && e.y<playerY+30 && e.y+e.h>playerY) {
                    playerHealth--; invincible=25; updateUI();
                    enemies.splice(i,1); i--;
                    if(playerHealth<=0) gameOver=true;
                    continue;
                }
                if(e.y>H+80) { enemies.splice(i,1); i--; }
            }
            if(invincible>0) invincible--;
            
            for(let i=0;i<bullets.length;i++) {
                let b=bullets[i]; b.y-=6.5;
                if(b.y+b.h<0 || b.y>H) { bullets.splice(i,1); i--; continue; }
                let hit=false;
                for(let j=0;j<enemies.length;j++) {
                    let e=enemies[j];
                    if(b.x<e.x+e.w && b.x+b.w>e.x && b.y<e.y+e.h && b.y+b.h>e.y) {
                        e.hp--; hit=true;
                        if(e.hp<=0) { enemies.splice(j,1); score+=10; kills++; updateUI(); }
                        break;
                    }
                }
                if(hit) { bullets.splice(i,1); i--; }
            }
            frame++;
            if(playerHealth<=0) gameOver=true;
        }
        
        function draw() {
            ctx.clearRect(0,0,W,H);
            ctx.fillStyle="#0a1020"; ctx.fillRect(0,0,W,H);
            for(let e of enemies) {
                ctx.fillStyle=e.color; ctx.fillRect(e.x,e.y,e.w,e.h);
                ctx.fillStyle="#ff4444"; ctx.fillRect(e.x+5,e.y+6,6,5); ctx.fillRect(e.x+e.w-11,e.y+6,6,5);
                ctx.fillStyle="#000"; ctx.fillRect(e.x+6,e.y+7,2,2); ctx.fillRect(e.x+e.w-10,e.y+7,2,2);
                if(e.hp>1) { ctx.fillStyle="#ffaa55"; ctx.font="bold 9px monospace"; ctx.fillText("❤️"+e.hp, e.x+10, e.y+22); }
            }
            for(let b of bullets) { ctx.fillStyle="#ffdd77"; ctx.fillRect(b.x,b.y,b.w,b.h); }
            let a=(invincible>0 && (Date.now()/60)%3<1)?0.5:1;
            ctx.fillStyle=`rgba(70,130,200,${a})`; ctx.fillRect(playerX,playerY,32,30);
            ctx.fillStyle=`rgba(200,220,250,${a})`; ctx.beginPath(); ctx.arc(playerX+16,playerY+15,10,0,Math.PI*2); ctx.fill();
            if(gameOver){ ctx.fillStyle="rgba(0,0,0,0.8)"; ctx.fillRect(0,0,W,H); ctx.fillStyle="#ffaa77"; ctx.font="bold 20px monospace"; ctx.fillText("GAME OVER", W/2-80, H/2); }
        }
        
        function move() {
            if(gameOver) return;
            if(leftPress) playerX-=5.2;
            if(rightPress) playerX+=5.2;
            if(playerX<0) playerX=0;
            if(playerX+32>W) playerX=W-32;
        }
        
        document.getElementById('shooterLeft').onmousedown = ()=>leftPress=true;
        document.getElementById('shooterLeft').onmouseup = ()=>leftPress=false;
        document.getElementById('shooterLeft').ontouchstart = (e)=>{e.preventDefault(); leftPress=true;};
        document.getElementById('shooterLeft').ontouchend = ()=>leftPress=false;
        document.getElementById('shooterRight').onmousedown = ()=>rightPress=true;
        document.getElementById('shooterRight').onmouseup = ()=>rightPress=false;
        document.getElementById('shooterRight').ontouchstart = (e)=>{e.preventDefault(); rightPress=true;};
        document.getElementById('shooterRight').ontouchend = ()=>rightPress=false;
        document.getElementById('shooterShoot').onclick = ()=>shoot();
        document.getElementById('shooterShoot').ontouchstart = (e)=>{e.preventDefault(); shoot();};
        document.getElementById('shooterReset').onclick = ()=>{
            gameOver=false; score=0; kills=0; playerHealth=5; enemies=[]; bullets=[]; invincible=0; fireDelay=0; updateUI();
        };
        
        function loop() { move(); updateGame(); draw(); requestAnimationFrame(loop); }
        updateUI(); loop();
    }
    
    // --------------------------------------------------------------
    // 2. ИГРА: 3D КОЛЛЕКТОР (упрощённая)
    // --------------------------------------------------------------
    function createCollectorGame(containerId) {
        const container = document.getElementById(containerId);
        container.innerHTML = `
            <div class="game-header"><h2>✨ 3D КОЛЛЕКТОР</h2></div>
            <div class="game-desc">Управляй шаром, собирай кристаллы, избегай препятствий.</div>
            <div class="game-canvas-container"><canvas id="collectorCanvas" width="400" height="400"></canvas></div>
            <div class="game-stats"><span>💎 КРИСТАЛЛЫ: <span id="collectorScore">0</span></span><span>❤️ ЖИЗНИ: <span id="collectorHealth">3</span></span></div>
            <div class="game-controls">
                <button class="ctrl-btn" id="collectorLeft">◀ ЛЕВО</button>
                <button class="ctrl-btn" id="collectorRight">ПРАВО ▶</button>
                <button class="ctrl-btn reset-game-btn" id="collectorReset">🔄 РЕСТАРТ</button>
            </div>
        `;
        
        const canvas = document.getElementById('collectorCanvas');
        const ctx = canvas.getContext('2d');
        const W=400, H=400;
        canvas.width=W; canvas.height=H;
        
        let ballX=W/2, score=0, health=3, gameOver=false;
        let crystals=[], obstacles=[];
        let left=false, right=false;
        
        function updateUIc() {
            document.getElementById('collectorScore').innerText = score;
            document.getElementById('collectorHealth').innerText = health;
        }
        
        function spawnCrystal() { crystals.push({ x: 20+Math.random()*(W-40), y: -15, r:8 }); }
        function spawnObstacle() { obstacles.push({ x: 20+Math.random()*(W-40), y: -20, w:22, h:22 }); }
        
        function resetGamec() {
            gameOver=false; score=0; health=3; ballX=W/2; crystals=[]; obstacles=[];
            for(let i=0;i<4;i++) spawnCrystal();
            for(let i=0;i<3;i++) spawnObstacle();
            updateUIc();
        }
        
        function updateGamec() {
            if(gameOver) return;
            if(left && ballX>12) ballX-=6;
            if(right && ballX<W-12) ballX+=6;
            for(let i=0;i<crystals.length;i++) {
                crystals[i].y+=2.5;
                if(crystals[i].y+12>0 && Math.hypot(ballX-crystals[i].x, 370-crystals[i].y)<18) {
                    crystals.splice(i,1); score++; updateUIc(); spawnCrystal();
                    i--;
                }
                if(crystals[i].y>H+30) { crystals.splice(i,1); spawnCrystal(); i--; }
            }
            for(let i=0;i<obstacles.length;i++) {
                obstacles[i].y+=2.2;
                if(obstacles[i].y+22>0 && obstacles[i].x<ballX+12 && obstacles[i].x+22>ballX-12 && obstacles[i].y+22>360 && obstacles[i].y<380) {
                    health--; updateUIc(); obstacles.splice(i,1); spawnObstacle(); i--;
                    if(health<=0) gameOver=true;
                    continue;
                }
                if(obstacles[i].y>H+50) { obstacles.splice(i,1); spawnObstacle(); i--; }
            }
            if(crystals.length<3) spawnCrystal();
            if(obstacles.length<2) spawnObstacle();
        }
        
        function drawc() {
            ctx.clearRect(0,0,W,H);
            ctx.fillStyle="#0a1425"; ctx.fillRect(0,0,W,H);
            for(let c of crystals) { ctx.fillStyle="#88ddff"; ctx.beginPath(); ctx.arc(c.x, c.y, 8, 0, Math.PI*2); ctx.fill(); ctx.fillStyle="#ffffff"; ctx.beginPath(); ctx.arc(c.x-2, c.y-2, 2, 0, Math.PI*2); ctx.fill(); }
            for(let o of obstacles) { ctx.fillStyle="#aa5566"; ctx.fillRect(o.x, o.y, o.w, o.h); }
            ctx.fillStyle="#ffaa66"; ctx.beginPath(); ctx.arc(ballX, 370, 12, 0, Math.PI*2); ctx.fill();
            if(gameOver){ ctx.fillStyle="rgba(0,0,0,0.7)"; ctx.fillRect(0,0,W,H); ctx.fillStyle="#ffaa77"; ctx.font="bold 20px monospace"; ctx.fillText("GAME OVER", W/2-80, H/2); }
        }
        
        document.getElementById('collectorLeft').onmousedown = ()=>left=true;
        document.getElementById('collectorLeft').onmouseup = ()=>left=false;
        document.getElementById('collectorLeft').ontouchstart = (e)=>{e.preventDefault(); left=true;};
        document.getElementById('collectorLeft').ontouchend = ()=>left=false;
        document.getElementById('collectorRight').onmousedown = ()=>right=true;
        document.getElementById('collectorRight').onmouseup = ()=>right=false;
        document.getElementById('collectorRight').ontouchstart = (e)=>{e.preventDefault(); right=true;};
        document.getElementById('collectorRight').ontouchend = ()=>right=false;
        document.getElementById('collectorReset').onclick = ()=>{ resetGamec(); };
        
        resetGamec();
        function loopc() { updateGamec(); drawc(); requestAnimationFrame(loopc); }
        loopc();
    }
    
    // --------------------------------------------------------------
    // 3. ИГРА: КОСМИЧЕСКИЙ ЗАЩИТНИК (ракеты)
    // --------------------------------------------------------------
    function createDefenderGame(containerId) {
        const container = document.getElementById(containerId);
        container.innerHTML = `
            <div class="game-header"><h2>🛸 КОСМИЧЕСКИЙ ЗАЩИТНИК</h2></div>
            <div class="game-desc">Сбивай вражеские корабли ракетами. Тапай по экрану или жми ОГОНЬ!</div>
            <div class="game-canvas-container"><canvas id="defenderCanvas" width="400" height="400"></canvas></div>
            <div class="game-stats"><span>🎯 СБИТО: <span id="defenderScore">0</span></span><span>🛡️ БАЗА: <span id="defenderHealth">10</span></span></div>
            <div class="game-controls"><button class="ctrl-btn shoot-btn" id="defenderShoot">🚀 ВЫСТРЕЛ</button><button class="ctrl-btn reset-game-btn" id="defenderReset">🔄 РЕСТАРТ</button></div>
        `;
        const canvas = document.getElementById('defenderCanvas');
        const ctx = canvas.getContext('2d');
        const W=400, H=400;
        canvas.width=W; canvas.height=H;
        let enemies=[], missiles=[], score=0, baseHp=10, gameOver=false, shootDelay=0;
        
        function updateUId() {
            document.getElementById('defenderScore').innerText = score;
            document.getElementById('defenderHealth').innerText = baseHp;
            if(baseHp<=0) gameOver=true;
        }
        function spawnEnemy() { enemies.push({ x:Math.random()*(W-28), y:-28, w:28, h:28, hp:1, speed:1.4+Math.random() }); }
        function shootRocket() {
            if(gameOver||shootDelay>0) return;
            missiles.push({ x:W/2-4, y:H-35, w:8, h:12, target:null });
            shootDelay=25;
        }
        function updateDef() {
            if(gameOver) return;
            if(shootDelay>0) shootDelay--;
            if(Math.random()<0.025 && enemies.length<7) spawnEnemy();
            for(let i=0;i<enemies.length;i++) {
                let e=enemies[i]; e.y+=e.speed;
                if(e.y+e.h>H-25 && e.x+10>W/2-30 && e.x<W/2+30) { baseHp--; updateUId(); enemies.splice(i,1); i--; continue; }
                if(e.y>H+60) { enemies.splice(i,1); i--; }
            }
            for(let i=0;i<missiles.length;i++) {
                let m=missiles[i]; m.y-=5.5;
                if(m.y+m.h<0) { missiles.splice(i,1); i--; continue; }
                let hit=false;
                for(let j=0;j<enemies.length;j++) {
                    let e=enemies[j];
                    if(m.x<e.x+e.w && m.x+m.w>e.x && m.y<e.y+e.h && m.y+m.h>e.y) {
                        enemies.splice(j,1); score++; updateUId(); hit=true; break;
                    }
                }
                if(hit) { missiles.splice(i,1); i--; }
            }
        }
        function drawDef() {
  
