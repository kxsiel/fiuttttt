# fiuttttt

<!DOCTYPE html>

<html lang="pl">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>xyz</title>

    <script src="https://cdn.tailwindcss.com"></script>

    <link href="https://fonts.googleapis.com/css2?family=Space+Mono:ital,wght@0,400;0,700;1,400&family=Syncopate:wght@700&display=swap" rel="stylesheet">

    <link href="https://fonts.cdnfonts.com/css/akira-expanded" rel="stylesheet">

    

    <style>

        :root {

            --bg: #030303;

            --fg: #f4f4f4;

            --accent: #ff003c;

            --grid: rgba(255, 255, 255, 0.03);

        }



        * { margin: 0; padding: 0; box-sizing: border-box; cursor: none !important; }



        body {

            background-color: var(--bg);

            color: var(--fg);

            font-family: 'Space Mono', monospace;

            overflow: hidden;

            height: 100vh;

            perspective: 1500px;

        }



        /* --- HARDCORE CUSTOM CURSOR --- */

        /* Usunięto top/left z CSS, dodano will-change dla wydajności */

        #cursor-core {

            width: 4px; height: 4px; background: #fff; border-radius: 50%;

            position: fixed; pointer-events: none; z-index: 10000;

            top: 0; left: 0;

            mix-blend-mode: difference;

            will-change: transform;

        }

        #cursor-ring {

            width: 40px; height: 40px;

            border: 1px dashed rgba(255,255,255,0.5);

            border-radius: 50%; position: fixed; pointer-events: none; z-index: 9999;

            top: 0; left: 0;

            transition: width 0.2s, height 0.2s;

            will-change: transform;

        }

        #cursor-trail {

            width: 12px; height: 12px; border: 1px solid #fff;

            position: fixed; pointer-events: none; z-index: 9998;

            top: 0; left: 0;

            mix-blend-mode: difference;

            will-change: transform;

        }



        /* --- BACKGROUND CHAOS --- */

        .crt-overlay {

            position: fixed; inset: 0; z-index: 9000; pointer-events: none;

            background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.25) 50%), linear-gradient(90deg, rgba(255, 0, 0, 0.06), rgba(0, 255, 0, 0.02), rgba(0, 0, 255, 0.06));

            background-size: 100% 3px, 3px 100%;

        }

        

        .noise {

            position: fixed; inset: -50%; width: 200%; height: 200%; z-index: 8999;

            pointer-events: none; opacity: 0.12;

            background: url('https://upload.wikimedia.org/wikipedia/commons/7/76/1k_Noise_Texture.png');

            animation: noise-jerk 0.05s infinite;

        }

        @keyframes noise-jerk {

            0% { transform: translate(0, 0); }

            20% { transform: translate(-5px, 5px); }

            40% { transform: translate(-5px, -5px); }

            60% { transform: translate(5px, 5px); }

            80% { transform: translate(5px, -5px); }

            100% { transform: translate(0, 0); }

        }



        .grid-bg {

            position: fixed; top: -50%; left: -50%; width: 200%; height: 200%;

            background-image: linear-gradient(var(--grid) 1px, transparent 1px), linear-gradient(90deg, var(--grid) 1px, transparent 1px);

            background-size: 50px 50px; z-index: -2;

            transform: rotateX(60deg) translateY(0);

            will-change: transform;

        }



        .marquee-container {

            position: absolute; top: 20%; left: -10%; width: 150%;

            font-family: 'Akira Expanded', sans-serif; font-size: 12vw;

            color: transparent; -webkit-text-stroke: 1px rgba(255,255,255,0.03);

            white-space: nowrap; z-index: -1; opacity: 0.5;

            will-change: transform;

        }



        /* --- BOOT SEQUENCE --- */

        #boot-screen {

            position: fixed; inset: 0; background: #000; z-index: 100000;

            display: flex; flex-direction: column; justify-content: flex-end; padding: 40px;

        }

        .boot-text { color: #fff; font-size: 12px; margin-bottom: 5px; opacity: 0.8; font-weight: bold; }

        

        #enter-wrapper {

            display: none; align-self: center; margin-top: auto; margin-bottom: auto; text-align: center;

        }

        .enter-btn {

            font-family: 'Akira Expanded', sans-serif; font-size: 2rem;

            color: transparent; -webkit-text-stroke: 1px #fff;

            padding: 15px 30px; border: 1px solid rgba(255,255,255,0.2);

            position: relative; overflow: hidden; transition: 0.3s;

            display: inline-block;

        }

        .enter-btn::before {

            content: 'CLICK TO PROCEED'; position: absolute; left: 0; top: 0; padding: 15px 30px;

            color: var(--bg); width: 0%; overflow: hidden; white-space: nowrap; transition: width 0.4s cubic-bezier(0.8, 0, 0.2, 1);

            background: #fff; z-index: -1;

        }

        .enter-btn:hover { border-color: #fff; letter-spacing: 4px; text-shadow: 0 0 10px rgba(255,255,255,0.5); }

        .enter-btn:hover::before { width: 100%; }



        /* --- HUD ELEMENTS --- */

        .hud { position: fixed; font-size: 10px; color: rgba(255,255,255,0.4); z-index: 100; pointer-events: none; }

        .hud-tl { top: 20px; left: 20px; }

        .hud-tr { top: 20px; right: 20px; text-align: right; }

        .hud-bl { bottom: 20px; left: 20px; }

        .hud-br { bottom: 20px; right: 20px; text-align: right; }

        

        .crosshair-corner {

            position: absolute; width: 15px; height: 15px; border: 1px solid #fff; z-index: 101; pointer-events: none;

        }

        .cc-tl { top: 40px; left: 40px; border-right: none; border-bottom: none; }

        .cc-tr { top: 40px; right: 40px; border-left: none; border-bottom: none; }

        .cc-bl { bottom: 40px; left: 40px; border-right: none; border-top: none; }

        .cc-br { bottom: 40px; right: 40px; border-left: none; border-top: none; }



        /* --- MAIN CARDS (BRUTALISM) --- */

        .wrapper {

            position: relative; width: 100%; height: 100%;

            display: flex; align-items: center; justify-content: center; z-index: 10;

            will-change: transform;

        }



        .brutal-box {

            background: rgba(5, 5, 5, 0.9);

            border: 1px solid rgba(255, 255, 255, 0.1);

            position: relative;

            backdrop-filter: blur(5px);

            transform-style: preserve-3d;

            transition: border-color 0.3s;

        }



        .brutal-box::before {

            content: ''; position: absolute; inset: -1px; background: #fff; z-index: -1;

            clip-path: polygon(0 0, 10px 0, 10px 1px, 1px 1px, 1px 10px, 0 10px);

        }




        .brutal-shadow {

            position: absolute;

            top: 10px; left: 10px; right: -10px; bottom: -10px;

            border: 1px solid rgba(255, 255, 255, 0.05);

            background: repeating-linear-gradient(45deg, transparent, transparent 2px, rgba(255,255,255,0.02) 2px, rgba(255,255,255,0.02) 4px);

            z-index: -2;

            transition: all 0.3s ease;

        }

        .brutal-box:hover .brutal-shadow {

            top: 15px; left: 15px; right: -15px; bottom: -15px;

            border-color: rgba(255, 0, 60, 0.3);

            background: repeating-linear-gradient(45deg, transparent, transparent 2px, rgba(255,0,60,0.05) 2px, rgba(255,0,60,0.05) 4px);

        }

        .brutal-box:hover { border-color: rgba(255, 255, 255, 0.5); }



        .avatar-container { position: relative; width: 120px; height: 120px; margin: 0 auto; overflow: hidden; border: 1px solid #333; }

        .avatar-img { width: 100%; height: 100%; object-fit: cover; filter: grayscale(100%) contrast(150%); transition: filter 0.3s; }

        .avatar-container:hover .avatar-img { animation: rgb-split 0.2s infinite alternate; }

        

        @keyframes rgb-split {

            0% { filter: drop-shadow(3px 0 0 red) drop-shadow(-3px 0 0 cyan) grayscale(100%); transform: translate(-2px, 1px); }

            100% { filter: drop-shadow(-3px 0 0 red) drop-shadow(3px 0 0 cyan) grayscale(100%); transform: translate(2px, -1px); }

        }



        .akira-text { font-family: 'Akira Expanded', sans-serif; text-transform: uppercase; }

        .glitch-hover:hover { color: #fff; animation: text-shake 0.2s infinite; text-shadow: 2px 0 var(--accent), -2px 0 cyan; }

        @keyframes text-shake { 0% { transform: skewX(-15deg); } 50% { transform: skewX(15deg); } 100% { transform: skewX(0); } }



        .barcode { font-family: 'Libre Barcode 39 Extended Text', cursive; font-size: 30px; opacity: 0.3; pointer-events: none; }



        .cyber-btn {

            background: #fff; color: #000; font-weight: bold; font-size: 10px; padding: 10px 20px;

            text-transform: uppercase; border: none; position: relative; overflow: hidden;

            clip-path: polygon(10px 0, 100% 0, 100% calc(100% - 10px), calc(100% - 10px) 100%, 0 100%, 0 10px);

            transition: all 0.2s;

        }

        .cyber-btn::before {

            content: ''; position: absolute; top: 0; left: -100%; width: 100%; height: 100%;

            background: repeating-linear-gradient(90deg, transparent, transparent 2px, rgba(0,0,0,0.2) 2px, rgba(0,0,0,0.2) 4px);

            transition: left 0.3s;

        }

        .cyber-btn:hover { background: #ccc; transform: scale(1.05); }

        .cyber-btn:hover::before { left: 0; }

        

        ::-webkit-scrollbar { display: none; }



        /* Klasy pomocnicze dla JS do ringa na hover */

        .ring-hover { width: 60px !important; height: 60px !important; border-color: var(--accent) !important; }

    </style>

</head>

<body>



    <div id="cursor-core"></div>

    <div id="cursor-ring"></div>

    <div id="cursor-trail"></div>



    <div id="boot-screen">

        <div id="terminal-logs"></div>

        <div id="enter-wrapper">

            <div class="text-[10px] text-white/50 mb-4 tracking-[0.3em] uppercase">Authenticator</div>

            <div class="enter-btn" onclick="initSystem()">CLICK TO PROCEED</div>

        </div>

    </div>



    <audio id="track" loop>

        <source src="trickydisco_5.wav" type="audio/wav">

    </audio>



    <div class="crt-overlay"></div>

    <div class="noise"></div>

    <div class="grid-bg" id="parallax-bg"></div>

    <div class="marquee-container" id="parallax-marquee">


    </div>



    <div class="crosshair-corner cc-tl"></div>

    <div class="crosshair-corner cc-tr"></div>

    <div class="crosshair-corner cc-bl"></div>

    <div class="crosshair-corner cc-br"></div>



    <div class="hud hud-tl">

        <div>SYS.OP: SUAXZ_CORE</div>

        <div>MEM: <span id="ram-usage">0</span>MB / 64000MB</div>

        <div class="mt-2 text-red-500 animate-pulse">REC (•)</div>

    </div>

    <div class="hud hud-tr">

        <div id="clock">00:00:00:00</div>

        <div>LOC: WARSAW_SECTOR_7</div>

        <div class="mt-2">LATENCY: <span id="ping">12</span>MS</div>

    </div>

    <div class="hud hud-bl" style="max-width: 200px;">

        <div id="live-log">INIT_OK.</div>

        <div class="barcode">madebysuaxz</div>

    </div>

    <div class="hud hud-br">

        <div>[X: <span id="coord-x">000</span> Y: <span id="coord-y">000</span>]</div>

        <div>AXIS_LOCK: OFF</div>

    </div>



    <div class="wrapper" id="parallax-wrapper">

        <div class="w-full max-w-[550px] p-4 flex flex-col gap-4 relative" id="main-cards">

            

            <div class="brutal-box p-10 flex flex-col items-center text-center">

	 <div class="absolute top-2 left-2 text-[8px] opacity-30">[ID: 0x8F2A]</div>

                <div class="absolute top-2 right-2 text-[8px] opacity-30">[STATUS: ACTIVE]</div>

                

                <div class="avatar-container mb-6 border-white/20">

                    <img src="https://i.pinimg.com/736x/0f/5e/9b/0f5e9b0e20f84af6394519d21f41e792.jpg" class="avatar-img" alt="avatar">

                    <div class="absolute bottom-0 w-full bg-black/90 text-center text-[7px] py-1 border-t border-[#333]">
                    </div>

                </div>



                <h1 class="akira-text text-5xl tracking-widest mb-2 glitch-hover" data-value="SUAXZ">SUAXZ</h1>

                

                <div class="flex items-center gap-2 text-[9px] tracking-[0.5em] text-white/40 uppercase mt-4">

                    <div class="w-2 h-2 bg-white animate-ping"></div>

                    DIGITAL_ENTITY // 2026

                </div>

            </div>



            <div class="brutal-box p-6 flex justify-between items-center group cursor-pointer" onmouseover="scrambleText(this.querySelector('.scramble-target'))">

                <div class="brutal-shadow"></div> <div class="flex flex-col gap-1">

                    <div class="text-[8px] text-white/30">MODULE_01</div>

                    <div class="text-sm font-bold tracking-[0.3em] uppercase scramble-target group-hover:text-white transition-colors">UPCOMING_PROJECT</div>

                </div>

                <div class="text-right">

                    <div class="text-[10px] font-bold">ST@S.ONL</div>

                    <div class="text-[8px] text-white/30">ENCRYPTED</div>

                </div>

            </div>



            <div class="brutal-box p-6 flex justify-between items-center group">

                <div class="brutal-shadow"></div> <div class="flex items-center gap-5">

                    <div class="w-12 h-12 border border-white/20 bg-white/5 flex items-center justify-center relative overflow-hidden group-hover:border-white transition-colors">

                        <span class="akira-text text-lg z-10">D</span>

                        <div class="absolute inset-0 bg-white/20 translate-y-full group-hover:translate-y-0 transition-transform duration-300"></div>

                    </div>

                    <div class="flex flex-col gap-1" onmouseover="scrambleText(this.querySelector('.scramble-target'))">

                        <div class="text-[8px] text-white/30">MODULE_02 // COMM_LINK</div>

                        <div class="text-sm font-bold tracking-[0.2em] uppercase scramble-target">DISCORD_SERVER</div>

                        <div class="text-[8px] text-green-500">>>> ??? USERS ONLINE</div>

                    </div>

                </div>

                <a href="https://discord.gg/p4Gpg9Cpam" target="_blank" class="z-20 relative hover-trigger">

                    <button class="cyber-btn">J O I N</button>

                </a>

            </div>



            <div class="brutal-box p-5 mt-2 flex flex-col relative overflow-hidden group">

                <div class="brutal-shadow"></div>

                <div class="absolute top-2 right-2 text-[7px] text-white/30 tracking-[0.2em]">AUDIO_SYS_V2</div>

                

                <div class="flex items-center gap-5 mt-1">

                    <button id="play-btn" class="cyber-btn w-12 h-12 flex-shrink-0 flex items-center justify-center text-xs z-20 hover-trigger" onclick="toggleMusic()">

                        [ ► ]

                    </button>

                    

                    <div class="flex-col flex-1 w-full relative z-10">

                        <div class="flex justify-between text-[10px] font-bold tracking-[0.1em] mb-2 uppercase">

                            <span class="scramble-target glitch-hover cursor-pointer" onmouseover="scrambleText(this)">TRICKY_DISCO_.WAV</span>

                            <span id="time-display" class="text-white/50">00:00</span>

                        </div>

                        

                        <div class="w-full h-[3px] bg-white/10 relative group-hover:bg-white/20 transition-colors cursor-pointer" onclick="seekMusic(event)" id="progress-container">

                            <div id="progress-bar" class="absolute top-0 left-0 h-full bg-white shadow-[0_0_8px_#fff] w-0 transition-all duration-75 pointer-events-none"></div>

                        </div>

                        

                        <div class="flex justify-between items-end mt-3">

                            <div class="flex gap-[3px] h-4 flex-1 items-end mr-4" id="visualizer"></div>

                            

                            <div class="flex items-center gap-2 group/vol cursor-pointer w-[80px]" id="volume-container" onclick="changeVolume(event)">

                                <span class="text-[7px] text-white/50 tracking-widest mt-[2px]">VOL</span>

                                <div class="h-[2px] w-full bg-white/10 relative transition-colors group-hover/vol:bg-white/20">

                                    <div id="volume-bar" class="absolute top-0 left-0 h-full bg-[var(--accent)] shadow-[0_0_5px_var(--accent)] w-[40%] pointer-events-none"></div>

                                </div>

                            </div>

                        </div>

                    </div>

                </div>

            </div>



            <div class="flex justify-between w-full text-[8px] opacity-30 mt-2 tracking-widest uppercase">

                <span>> SYS.HALT // NULL</span>

                <span>BUILD 2026.4 © SUAXZ</span>

            </div>



        </div>

    </div>



    <script>

        /* --- 1. BOOT SEQUENCE --- */

        const logs = [

            "INITIALIZING NEURAL LINK... [OK]",

            "ESTABLISHING SECURE CONNECTION... [DONE]",

            "BYPASSING MAINFRAME FIREWALLS... [██████░░░░] 60%",

            "BYPASSING MAINFRAME FIREWALLS... [██████████] 100%",

            "DECRYPTING CORE FILES...",

            "LOADING UI MODULES... [IN PROGRESS]",

            "WARNING: UNREGISTERED USER DETECTED.",

            "> OVERRIDING PROTOCOL...",

            "SYSTEM READY. AWAITING MANUAL INPUT."

        ];

        

        let logIndex = 0;

        let isPlaying = false; // Przeniesiono na górę

        

        const terminal = document.getElementById('terminal-logs');

        const enterWrapper = document.getElementById('enter-wrapper');

        const track = document.getElementById('track');

        const playBtn = document.getElementById('play-btn');



        function typeLog() {

            if (logIndex < logs.length) {

                const p = document.createElement('div');

                p.className = 'boot-text';

                p.innerText = "> " + logs[logIndex];

                terminal.appendChild(p);

                logIndex++;

                setTimeout(typeLog, Math.random() * 200 + 50);

            } else {

                setTimeout(() => { enterWrapper.style.display = 'block'; }, 400);

            }

        }

        window.onload = typeLog;



        function initSystem() {

            document.getElementById('boot-screen').style.opacity = '0';

            document.getElementById('boot-screen').style.pointerEvents = 'none';

            document.getElementById('boot-screen').style.transition = 'opacity 0.6s cubic-bezier(0.8, 0, 0.2, 1)';

            setTimeout(() => { document.getElementById('boot-screen').style.display = 'none'; }, 600);

            

            track.play().then(() => {

                isPlaying = true;

                playBtn.innerText = "[ || ]";

                playBtn.style.color = "var(--accent)";

                playBtn.style.background = "#fff"; 

            }).catch(e => console.log("User interaction required for audio."));

            

            // Inicjalizacja głośności

            track.volume = 0.4;

        }



        /* --- 2. SMOOTH CURSOR & PARALLAX (Zoptymalizowane) --- */

        const cursorCore = document.getElementById('cursor-core');

        const cursorRing = document.getElementById('cursor-ring');

        const cursorTrail = document.getElementById('cursor-trail');

        const coordX = document.getElementById('coord-x');

        const coordY = document.getElementById('coord-y');

        

        const wrapper = document.getElementById('parallax-wrapper');

        const bgGrid = document.getElementById('parallax-bg');

        const marquee = document.getElementById('parallax-marquee');



        // Zmienne do płynnej animacji (lerp)

        let mouseX = window.innerWidth / 2;

        let mouseY = window.innerHeight / 2;

        let trailX = mouseX, trailY = mouseY;

        let pxX = mouseX, pxY = mouseY;



        // Czysta funkcja lerp (płynne podążanie)

        const lerp = (start, end, factor) => start + (end - start) * factor;



        document.addEventListener('mousemove', (e) => {

            mouseX = e.clientX;

            mouseY = e.clientY;

            

            // Szybki update HUD

            coordX.innerText = String(mouseX).padStart(4, '0');

            coordY.innerText = String(mouseY).padStart(4, '0');

        });



        // Obsługa styli hover dla customowego kursora

        document.querySelectorAll('.hover-trigger, .cyber-btn, .glitch-hover').forEach(el => {

            el.addEventListener('mouseenter', () => cursorRing.classList.add('ring-hover'));

            el.addEventListener('mouseleave', () => cursorRing.classList.remove('ring-hover'));

        });



        // Pętla renderująca dla najwyższej płynności (60FPS+)

        function renderLoop() {

            // Natychmiastowy kursor głowny

            cursorCore.style.transform = `translate3d(calc(${mouseX}px - 50%), calc(${mouseY}px - 50%), 0)`;

            cursorRing.style.transform = `translate3d(calc(${mouseX}px - 50%), calc(${mouseY}px - 50%), 0) rotate(${Date.now() / 10}deg)`; // Ring dalej rotuje

            

            // Opóźniony (lerpowany) kursor trail

            trailX = lerp(trailX, mouseX, 0.3);

            trailY = lerp(trailY, mouseY, 0.3);

            cursorTrail.style.transform = `translate3d(calc(${trailX}px - 50%), calc(${trailY}px - 50%), 0)`;



            // Lerpowana paralaksa

            pxX = lerp(pxX, mouseX, 0.05);

            pxY = lerp(pxY, mouseY, 0.05);



            const cx = window.innerWidth / 2;

            const cy = window.innerHeight / 2;

            const dx = (pxX - cx) / cx;

            const dy = (pxY - cy) / cy;



            wrapper.style.transform = `perspective(1000px) rotateX(${-dy * 15}deg) rotateY(${dx * 15}deg) translateZ(20px)`;

            bgGrid.style.transform = `rotateX(60deg) translateX(${-dx * 50}px)`; // Usunięto animację w CSS dla bgGrid, teraz steruje tym JS

            

            // Ruch marquee w tle

            let timeOffset = Date.now() * 0.05;

            marquee.style.transform = `translateX(calc(-50% - ${timeOffset % 50}px)) translateY(${-dy * 30}px)`;



            requestAnimationFrame(renderLoop);

        }

        renderLoop();



        /* --- 3. TEXT SCRAMBLE HACK EFFECT --- */

        const letters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789@#$%&*";

        

        function scrambleText(element) {

            let iterations = 0;

            const originalText = element.dataset.value || element.innerText;

            if(!element.dataset.value) element.dataset.value = originalText;



            const interval = setInterval(() => {

                element.innerText = originalText.split("").map((letter, index) => {

                    if(index < iterations) return originalText[index];

                    return letters[Math.floor(Math.random() * letters.length)];

                }).join("");



                if(iterations >= originalText.length) clearInterval(interval);

                iterations += 0.5; // Przyspieszono minimalnie

            }, 30);

        }



        /* --- 4. HUD LIVE UPDATES --- */

        setInterval(() => {

            document.getElementById('ram-usage').innerText = Math.floor(Math.random() * (48000 - 45000) + 45000);

            document.getElementById('ping').innerText = Math.floor(Math.random() * 5 + 9);

            

            const d = new Date();

            document.getElementById('clock').innerText = 

                `${String(d.getHours()).padStart(2, '0')}:${String(d.getMinutes()).padStart(2, '0')}:${String(d.getSeconds()).padStart(2, '0')}:${String(Math.floor(d.getMilliseconds()/10)).padStart(2,'0')}`;

        }, 50);



        const liveLogsList = [

            "SCANNING PORTS...", "PACKET LOSS: 0%", "REFRESHING NODES", "ROUTING SIGNAL", "DECRYPT_ERR_IGNORE", "UPLINK ESTABLISHED"

        ];

        setInterval(() => {

            const logElement = document.getElementById('live-log');

            logElement.innerText = "> " + liveLogsList[Math.floor(Math.random() * liveLogsList.length)];

            logElement.style.opacity = '1';

            setTimeout(() => logElement.style.opacity = '0.5', 200);

        }, 2500);



        /* --- 5. MUSIC PLAYER SYSTEM (UPGRADED) --- */

        const timeDisplay = document.getElementById('time-display');

        const progressBar = document.getElementById('progress-bar');

        const progressContainer = document.getElementById('progress-container');

        const visContainer = document.getElementById('visualizer');

        const volContainer = document.getElementById('volume-container');

        const volBar = document.getElementById('volume-bar');



        for(let i=0; i<32; i++) {

            let bar = document.createElement('div');

            bar.className = 'flex-1 bg-white/30 transition-all duration-75';

            bar.style.height = '2px';

            visContainer.appendChild(bar);

        }



        function toggleMusic() {

            if(isPlaying) {

                track.pause();

                playBtn.innerText = "[ ► ]";

                playBtn.style.color = "#000";

                playBtn.style.background = "#fff";

            } else {

                track.play();

                playBtn.innerText = "[ || ]";

                playBtn.style.color = "var(--accent)";

                playBtn.style.background = "#fff";

            }

            isPlaying = !isPlaying;

        }



        track.addEventListener('timeupdate', () => {

            if(track.duration) {

                const percent = (track.currentTime / track.duration) * 100;

                progressBar.style.width = percent + "%";

                

                let m = Math.floor(track.currentTime / 60);

                let s = Math.floor(track.currentTime % 60);

                timeDisplay.innerText = `${String(m).padStart(2,'0')}:${String(s).padStart(2,'0')}`;

            }

        });



        function seekMusic(e) {

            if(track.duration) {

                const rect = progressContainer.getBoundingClientRect();

                const clickX = e.clientX - rect.left;

                const width = rect.width;

                track.currentTime = (clickX / width) * track.duration;

            }

        }



        /* --- 6. VOLUME CONTROL --- */

        function changeVolume(e) {

            const rect = volContainer.getBoundingClientRect();

            const clickX = e.clientX - rect.left;

            let newVol = clickX / rect.width;

            

            // Ograniczenia 0.0 - 1.0

            if(newVol < 0) newVol = 0;

            if(newVol > 1) newVol = 1;

            

            track.volume = newVol;

            volBar.style.width = (newVol * 100) + "%";

        }



        // Animacja wizualizera

        setInterval(() => {

            Array.from(visContainer.children).forEach((bar, i) => {

                if(isPlaying) {

                    let multiplier = Math.sin(i / 32 * Math.PI) * 16; 

                    let h = Math.floor(Math.random() * multiplier * track.volume * 2) + 2; // Głośność wpływa na skok pasków!

                    bar.style.height = h + 'px';

                    bar.style.backgroundColor = Math.random() > 0.85 ? 'var(--accent)' : 'rgba(255,255,255,0.8)';

                } else {

                    bar.style.height = '2px';

                    bar.style.backgroundColor = 'rgba(255,255,255,0.2)';

                }

            });

        }, 100);



    </script>

</body>

</html>
