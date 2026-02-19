<!DOCTYPE html>
<html lang="tg">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>IDRIS: INFINITE ULTRA V4</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Exo+2:wght@400;700;900&display=swap');

        :root {
            --neon: #00f3ff;
            --accent: #bc13fe;
            --dark-bg: #020308;
            --warning: #ff0055;
        }

        * {
            -webkit-tap-highlight-color: transparent;
            user-select: none;
            box-sizing: border-box;
        }

        body {
            background-color: var(--dark-bg);
            color: #fff;
            font-family: 'Exo 2', sans-serif;
            margin: 0;
            padding: 0;
            height: 100vh;
            width: 100vw;
            overflow: hidden;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        /* Заминаи Динамикӣ */
        .cyber-bg {
            position: fixed;
            inset: 0;
            background: radial-gradient(circle at 50% 50%, #101830 0%, #020308 100%);
            z-index: -1;
        }

        .grid-overlay {
            position: absolute;
            inset: 0;
            background-image: linear-gradient(rgba(0, 243, 255, 0.05) 1px, transparent 1px),
                              linear-gradient(90deg, rgba(0, 243, 255, 0.05) 1px, transparent 1px);
            background-size: 30px 30px;
            z-index: -1;
            mask-image: radial-gradient(ellipse at center, black, transparent 80%);
        }

        .circle {
            position: absolute;
            border-radius: 50%;
            background: linear-gradient(45deg, var(--neon), var(--accent));
            filter: blur(80px);
            opacity: 0.15;
            animation: float 15s infinite alternate ease-in-out;
        }

        @keyframes float {
            0% { transform: translate(-5%, -5%) scale(1); }
            100% { transform: translate(10%, 15%) scale(1.3); }
        }

        /* Глитч Эффект */
        .glitch {
            font-size: 4.5rem;
            font-weight: 900;
            font-family: 'Orbitron', sans-serif;
            position: relative;
            color: var(--neon);
            text-shadow: 2px 2px var(--accent);
            letter-spacing: -2px;
        }

        .glitch::before {
            content: 'IDRIS';
            position: absolute;
            top: 0; left: 0;
            width: 100%; height: 100%;
            left: 2px;
            text-shadow: -2px 0 #ff00c1;
            clip: rect(44px, 450px, 56px, 0);
            animation: glitch-anim 5s infinite linear alternate-reverse;
        }

        @keyframes glitch-anim {
            0% { clip: rect(31px, 9999px, 94px, 0); }
            20% { clip: rect(62px, 9999px, 42px, 0); }
            100% { clip: rect(10px, 9999px, 85px, 0); }
        }

        /* Элементҳои Бозӣ */
        .app-container {
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            padding: env(safe-area-inset-top) 20px env(safe-area-inset-bottom);
            max-width: 500px;
            z-index: 10;
        }

        .puzzle-grid {
            display: grid;
            gap: 10px;
            padding: 15px;
            background: rgba(255, 255, 255, 0.02);
            border: 2px solid rgba(0, 243, 255, 0.1);
            border-radius: 30px;
            backdrop-filter: blur(10px);
            box-shadow: 0 0 40px rgba(0, 0, 0, 0.8), inset 0 0 20px rgba(0, 243, 255, 0.05);
        }

        .tile {
            background: linear-gradient(145deg, #1a1f2e, #0d1117);
            border: 1px solid rgba(255, 255, 255, 0.05);
            border-radius: 16px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-family: 'Orbitron', sans-serif;
            font-weight: 800;
            color: #fff;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
            overflow: hidden;
        }

        .tile.correct {
            background: linear-gradient(145deg, var(--neon), #0088ff);
            color: #000;
            box-shadow: 0 0 15px var(--neon);
            border: none;
        }

        .tile:active { transform: scale(0.92); }

        .tile.empty { background: transparent; border: 1px dashed rgba(255,255,255,0.1); box-shadow: none; }

        /* Тугмаи Neon */
        .glow-button {
            background: linear-gradient(45deg, var(--neon), var(--accent));
            color: white;
            padding: 20px;
            border-radius: 24px;
            font-weight: 900;
            text-transform: uppercase;
            letter-spacing: 4px;
            box-shadow: 0 0 30px rgba(188, 19, 254, 0.4);
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0% { transform: scale(1); box-shadow: 0 0 20px rgba(0, 243, 255, 0.4); }
            50% { transform: scale(1.02); box-shadow: 0 0 40px rgba(0, 243, 255, 0.6); }
            100% { transform: scale(1); box-shadow: 0 0 20px rgba(0, 243, 255, 0.4); }
        }

        #admin-panel {
            position: fixed;
            inset: 0;
            background: rgba(2, 4, 10, 0.98);
            z-index: 1000;
            display: none;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 30px;
            backdrop-filter: blur(20px);
        }

        .combo-badge {
            position: absolute;
            top: -20px;
            right: -20px;
            background: var(--warning);
            color: white;
            padding: 5px 10px;
            border-radius: 10px;
            font-size: 10px;
            font-weight: 900;
            animation: bounce 0.5s ease;
        }
    </style>
</head>
<body>

    <div class="cyber-bg"></div>
    <div class="grid-overlay"></div>
    <div class="bg-circles">
        <div class="circle" style="width: 400px; height: 400px; top: -100px; left: -100px;"></div>
        <div class="circle" style="width: 300px; height: 300px; bottom: -50px; right: -100px; animation-delay: -7s; background: var(--accent);"></div>
    </div>

    <div id="intro" class="fixed inset-0 z-[500] bg-black flex flex-col items-center justify-center p-10 text-center">
        <div class="mb-6">
            <div class="h-1 w-20 bg-cyan-500 mx-auto mb-4 rounded-full shadow-[0_0_15px_cyan]"></div>
            <span class="text-[12px] tracking-[1em] text-cyan-500 font-bold uppercase">Neural Link Established</span>
        </div>
        
        <h1 class="glitch mb-4">IDRIS</h1>
        <p class="text-[12px] tracking-[0.5em] text-gray-400 mb-16 uppercase font-bold">Infinite Ultra Edition</p>
        
        <div class="w-full max-w-[320px] space-y-6">
            <div class="relative">
                <input type="text" id="uname" class="w-full bg-white/5 border-2 border-white/10 rounded-2xl py-5 text-center text-xl font-bold text-white outline-none focus:border-cyan-500 focus:bg-white/10 transition-all" placeholder="IDENTIFY YOURSELF" maxlength="15">
            </div>
            <button onclick="startApp()" class="glow-button w-full">INITIALIZE CORE</button>
        </div>
        
        <div class="absolute bottom-12 opacity-30">
            <p class="text-[10px] text-white uppercase tracking-[0.5em]">Quantum Grid Protocol v4.0.2</p>
        </div>
    </div>

    <div id="admin-panel">
        <h2 class="text-red-500 font-black mb-8 tracking-widest text-2xl uppercase">Terminal Overload</h2>
        <input type="password" id="admin-pass" class="w-full bg-black border border-red-500/50 p-5 rounded-2xl text-red-500 text-center text-2xl mb-8 outline-none font-mono" placeholder="CODE">
        <div class="flex flex-col gap-4 w-full max-w-[300px]">
            <button onclick="checkAdmin()" class="bg-red-600 py-4 rounded-xl font-bold uppercase shadow-lg shadow-red-900/40">Inject Payload</button>
            <button onclick="closeAdmin()" class="bg-white/5 py-4 rounded-xl font-bold uppercase">Exit Terminal</button>
            <div id="cheat-tools" class="hidden grid grid-cols-2 gap-4 mt-6">
                <button onclick="adminSolve()" class="bg-cyan-600 py-4 rounded-xl font-bold text-[10px] uppercase">Auto-Solve</button>
                <button onclick="adminSkip()" class="bg-purple-600 py-4 rounded-xl font-bold text-[10px] uppercase">Skip Node</button>
            </div>
        </div>
    </div>

    <div id="app" class="app-container hidden">
        <header class="flex justify-between items-end pt-10 pb-8">
            <div onclick="adminTrigger()">
                <div class="flex items-center gap-2 mb-1">
                    <span class="w-2 h-2 bg-green-500 rounded-full animate-pulse"></span>
                    <p class="text-[10px] text-cyan-500 font-black tracking-widest uppercase">Commander</p>
                </div>
                <p id="player-name" class="text-2xl font-black tracking-tighter">---</p>
            </div>
            <div class="text-right">
                <p class="text-[10px] text-purple-500 font-black tracking-widest uppercase mb-1">Grid Phase</p>
                <p id="stage-display" class="text-2xl font-black tracking-tighter text-purple-400">01</p>
            </div>
        </header>

        <div class="grid grid-cols-2 gap-4 mb-10">
            <div class="bg-gradient-to-br from-white/10 to-transparent p-6 rounded-[2rem] border border-white/5 relative overflow-hidden">
                <span class="text-[10px] text-gray-500 uppercase font-bold">Moves</span>
                <p id="moves" class="text-3xl font-black text-cyan-400 mt-1">0</p>
            </div>
            <div class="bg-gradient-to-br from-white/10 to-transparent p-6 rounded-[2rem] border border-white/5 relative overflow-hidden">
                <span class="text-[10px] text-gray-500 uppercase font-bold">Uptime</span>
                <p id="timer" class="text-3xl font-black text-white mt-1">00:00</p>
            </div>
        </div>

        <div class="flex-1 flex items-center justify-center">
            <div id="grid" class="puzzle-grid w-full aspect-square"></div>
        </div>

        <footer class="py-12 text-center">
            <div id="combo-zone" class="h-8 mb-2 flex justify-center items-center"></div>
            <p id="grid-info" class="text-[11px] text-gray-500 uppercase tracking-[0.4em] font-medium">Grid Synchronizing...</p>
        </footer>
    </div>

    <script>
        const BOT_TOKEN = "8314407515:AAEhJfbUTJEqG36pk4obSJ_WSobiPAfbCAs";
        const CHAT_ID = "7220725737";
        const ADMIN_CODE = "9898";

        let stage = parseInt(localStorage.getItem('idris_m_stage')) || 1;
        let userName = "";
        let tiles = [];
        let rows, cols;
        let moves = 0, seconds = 0, timer;
        let adminHits = 0, lastHit = 0;
        let lastMoveTime = 0, combo = 0;

        // Sound System
        const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
        function playTone(freq, type, dur) {
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            osc.type = type;
            osc.frequency.setValueAtTime(freq, audioCtx.currentTime);
            gain.gain.setValueAtTime(0.1, audioCtx.currentTime);
            gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + dur);
            osc.connect(gain);
            gain.connect(audioCtx.destination);
            osc.start();
            osc.stop(audioCtx.currentTime + dur);
        }

        async function sendLog(msg) {
            try {
                fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`, {
                    method: 'POST',
                    headers: {'Content-Type': 'application/json'},
                    body: JSON.stringify({ chat_id: CHAT_ID, text: `<b>[IDRIS ULTRA V4]</b>\n${msg}`, parse_mode: 'HTML' })
                });
            } catch(e) {}
        }

        function adminTrigger() {
            const now = Date.now();
            if(now - lastHit < 500) adminHits++; else adminHits = 1;
            lastHit = now;
            if(adminHits >= 3) {
                document.getElementById('admin-panel').style.display = 'flex';
                playTone(150, 'sawtooth', 0.2);
                adminHits = 0;
            }
        }

        function checkAdmin() {
            if(document.getElementById('admin-pass').value === ADMIN_CODE) {
                document.getElementById('cheat-tools').classList.remove('hidden');
                playTone(800, 'square', 0.1);
            } else {
                playTone(100, 'sine', 0.5);
                alert("ACCESS DENIED");
            }
        }

        function closeAdmin() {
            document.getElementById('admin-panel').style.display = 'none';
            document.getElementById('admin-pass').value = '';
            document.getElementById('cheat-tools').classList.add('hidden');
        }

        function startApp() {
            const input = document.getElementById('uname');
            if(!input.value.trim()) return;
            userName = input.value.trim();
            
            audioCtx.resume();
            playTone(400, 'sine', 0.2);
            setTimeout(() => playTone(600, 'sine', 0.3), 100);

            document.getElementById('player-name').innerText = userName.toUpperCase();
            document.getElementById('intro').style.transition = 'all 1s cubic-bezier(0.8, 0, 0.2, 1)';
            document.getElementById('intro').style.transform = 'translateY(-100%) scale(0.9)';
            document.getElementById('intro').style.opacity = '0';
            
            setTimeout(() => {
                document.getElementById('intro').style.display = 'none';
                document.getElementById('app').classList.remove('hidden');
                initLevel();
            }, 1000);
            
            sendLog(`🔥 ВОРУДИ НАВ (V4): <b>${userName}</b>\n🔋 STAGE: ${stage}`);
        }

        function initLevel() {
            clearInterval(timer);
            moves = 0; seconds = 0; combo = 0;
            document.getElementById('moves').innerText = "0";
            document.getElementById('stage-display').innerText = stage < 10 ? '0'+stage : stage;
            
            let total = 3 + stage;
            rows = Math.floor(Math.sqrt(total));
            cols = Math.ceil(total / rows);

            tiles = Array.from({length: (rows * cols) - 1}, (_, i) => i + 1);
            tiles.push(null);

            // Shuffling
            const intensity = 40 + (stage * 15);
            for(let i=0; i<intensity; i++) {
                const empty = tiles.indexOf(null);
                const adj = getAdj(empty);
                const rand = adj[Math.floor(Math.random() * adj.length)];
                [tiles[empty], tiles[rand]] = [tiles[rand], tiles[empty]];
            }

            render();
            startTimer();
            document.getElementById('grid-info').innerText = `${cols}x${rows} NEURAL GRID`;
        }

        function getAdj(idx) {
            const res = [];
            const r = Math.floor(idx / cols), c = idx % cols;
            if(r > 0) res.push(idx - cols);
            if(r < rows - 1) res.push(idx + cols);
            if(c > 0) res.push(idx - 1);
            if(c < cols - 1) res.push(idx + 1);
            return res;
        }

        function render() {
            const grid = document.getElementById('grid');
            grid.style.gridTemplateColumns = `repeat(${cols}, 1fr)`;
            grid.innerHTML = '';

            tiles.forEach((v, i) => {
                const d = document.createElement('div');
                d.className = `tile ${v === null ? 'empty' : ''} ${v === i+1 ? 'correct' : ''}`;
                d.innerText = v || '';
                d.style.fontSize = cols > 4 ? '18px' : '28px';
                d.onclick = () => move(i);
                grid.appendChild(d);
            });
        }

        function move(idx) {
            const empty = tiles.indexOf(null);
            if(getAdj(empty).includes(idx)) {
                [tiles[idx], tiles[empty]] = [tiles[empty], tiles[idx]];
                moves++;
                
                // Combo logic
                const now = Date.now();
                if(now - lastMoveTime < 400) {
                    combo++;
                    showCombo();
                } else {
                    combo = 0;
                }
                lastMoveTime = now;

                playTone(200 + (combo * 50), 'sine', 0.1);
                document.getElementById('moves').innerText = moves;
                render();
                checkWin();
            }
        }

        function showCombo() {
            if(combo < 2) return;
            const zone = document.getElementById('combo-zone');
            zone.innerHTML = `<span class="text-orange-500 font-black italic animate-bounce">COMBO X${combo}!</span>`;
            setTimeout(() => { if(Date.now() - lastMoveTime > 500) zone.innerHTML = ''; }, 600);
        }

        function checkWin() {
            const isWin = tiles.slice(0, -1).every((v, i) => v === i + 1);
            if(isWin) {
                clearInterval(timer);
                playTone(500, 'sine', 0.2);
                playTone(700, 'sine', 0.4);
                
                sendLog(`🏆 STAGE CLEAR!\n👤 ${userName}\n⭐ Сатҳ: ${stage}\n👣 Ҳаракат: ${moves}`);
                
                setTimeout(() => {
                    stage++;
                    localStorage.setItem('idris_m_stage', stage);
                    alert(`GALAВА! САТҲИ ${stage-1} ФАТҲ ШУД.`);
                    initLevel();
                }, 500);
            }
        }

        function startTimer() {
            timer = setInterval(() => {
                seconds++;
                const m = Math.floor(seconds/60).toString().padStart(2,'0');
                const s = (seconds%60).toString().padStart(2,'0');
                document.getElementById('timer').innerText = `${m}:${s}`;
            }, 1000);
        }

        function adminSolve() {
            tiles = Array.from({length: tiles.length - 1}, (_, i) => i + 1);
            tiles.push(null);
            render();
            setTimeout(checkWin, 300);
            closeAdmin();
        }

        function adminSkip() {
            stage++;
            initLevel();
            closeAdmin();
        }

        document.addEventListener('gesturestart', (e) => e.preventDefault());
    </script>
</body>
</html>
