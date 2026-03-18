<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>KIRMIZI_BUTON_KNK: Gökhan 19</title>
    <style>
        body, html {
            margin: 0;
            padding: 0;
            width: 100%;
            height: 100%;
            background-color: #030303;
            color: #00ff00;
            font-family: 'Consolas', 'Courier New', monospace;
            overflow: hidden;
            touch-action: manipulation;
        }

        .scanlines {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.25) 50%), linear-gradient(90deg, rgba(255, 0, 0, 0.06), rgba(0, 255, 0, 0.02), rgba(0, 0, 255, 0.06));
            background-size: 100% 4px, 3px 100%;
            z-index: 50;
            pointer-events: none;
        }

        #flash {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: #00ff00;
            z-index: 40;
            opacity: 0;
            pointer-events: none;
        }

        #terminal-ui {
            position: absolute;
            top: 30px;
            left: 20px;
            right: 20px;
            z-index: 10;
            font-size: 18px;
            line-height: 1.6;
            pointer-events: none;
            text-shadow: 0 0 5px #00ff00;
        }

        #final-message-container {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            z-index: 30;
            text-align: center;
            width: 95%;
            display: none;
        }

        #final-message {
            font-size: 7vw; 
            font-weight: 900;
            font-family: 'Arial Black', Impact, sans-serif;
            color: #00ff00;
            text-shadow: 0 0 20px #00ff00, 0 0 40px #00aa00;
            text-transform: uppercase;
            letter-spacing: 1px;
            margin: 0 0 15px 0;
            word-wrap: break-word;
        }

        /* FOTOĞRAF ÇERÇEVESİ VE EFEKTİ */
        #target-photo {
            max-height: 40vh; 
            width: auto;
            max-width: 95%;
            border: 3px solid #00ff00;
            box-shadow: 0 0 20px #00ff00, inset 0 0 15px #00ff00;
            display: none;
            margin: 0 auto;
            filter: grayscale(100%) sepia(100%) hue-rotate(70deg) brightness(1.2) contrast(1.5);
            animation: hologramFlicker 3s infinite;
            object-fit: contain;
            background-color: #002200;
            transition: opacity 0.2s; /* Resimler arası geçiş yumuşaklığı */
        }

        @keyframes hologramFlicker {
            0% { opacity: 1; filter: grayscale(100%) sepia(100%) hue-rotate(70deg) brightness(1.2) contrast(1.5) blur(0px); }
            5% { opacity: 0.8; filter: grayscale(100%) sepia(100%) hue-rotate(70deg) brightness(1.5) contrast(2) blur(1px); }
            10% { opacity: 1; filter: grayscale(100%) sepia(100%) hue-rotate(70deg) brightness(1.2) contrast(1.5) blur(0px); }
            50% { opacity: 1; }
            52% { opacity: 0.9; transform: skewX(1deg); }
            54% { opacity: 1; transform: skewX(0deg); }
            100% { opacity: 1; }
        }

        /* Fotoğraf Değişirken Çıkan Glitch Efekti */
        .glitch-transition {
            animation: glitchEffect 0.2s forwards !important;
        }

        @keyframes glitchEffect {
            0% { transform: skewX(0deg); opacity: 1; filter: brightness(1); }
            25% { transform: skewX(20deg); opacity: 0.5; filter: brightness(2) hue-rotate(90deg); }
            50% { transform: skewX(-20deg); opacity: 0.8; filter: brightness(0.5) blur(5px); }
            75% { transform: skewX(10deg); opacity: 0.3; filter: brightness(1.5) hue-rotate(-90deg); }
            100% { transform: skewX(0deg); opacity: 1; filter: brightness(1); }
        }

        #credit {
            position: absolute;
            bottom: 20px;
            right: 20px;
            z-index: 30;
            font-size: 16px;
            color: #00aa00;
            text-shadow: 0 0 5px #00aa00;
            display: none;
            font-family: 'Consolas', monospace;
        }

        .cursor {
            display: inline-block;
            width: 10px;
            height: 1.2em;
            background-color: #00ff00;
            animation: blink 1s step-end infinite;
            vertical-align: bottom;
            box-shadow: 0 0 8px #00ff00;
            margin-left: 5px;
        }

        @keyframes blink {
            0%, 100% { opacity: 1; }
            50% { opacity: 0; }
        }

        #canvas {
            display: block;
            width: 100%;
            height: 100%;
            position: absolute;
            top: 0;
            left: 0;
            z-index: 1;
        }

        #start-btn {
            position: absolute;
            top: 70%;
            left: 50%;
            transform: translate(-50%, -50%);
            padding: 15px 30px;
            background-color: #ff0000;
            border: 3px solid #ffffff;
            color: #ffffff;
            font-size: 24px;
            font-family: 'Consolas', 'Courier New', monospace;
            font-weight: bold;
            cursor: pointer;
            z-index: 20;
            text-transform: uppercase;
            box-shadow: none;
            border-radius: 0;
            pointer-events: auto;
            display: none;
        }

        #start-btn:active { background-color: #800000; color: #aaaaaa; border-color: #aaaaaa; }
    </style>
</head>
<body>

    <div class="scanlines"></div>
    <div id="flash"></div>

    <div id="terminal-ui">
        <div id="typewriter"></div>
    </div>
    
    <div id="final-message-container">
        <div id="final-message"></div>
        <img id="target-photo" alt="Gökhan ve Ekip">
    </div>

    <div id="credit"></div>

    <canvas id="canvas"></canvas>
    <button id="start-btn">[ KIRMIZI BUTON KNK ]</button>

    <script>
        // ====================================================================================
        // YUSUF, TÜM FOTOĞRAF LİNKLERİNİ BURAYA YAPIŞTIR
        // Diğer fotoğrafları da hızlıresim'e yükleyip linklerini aşağıya sırayla ekle!
        // ====================================================================================
        const HAM_FOTOGRAFLAR = [
            "https://i.hizliresim.com/8iqb8sz.jpeg", // 1. FOTOĞRAF 
            "https://i.hizliresim.com/7tlgo1q.jpeg",                // 2. FOTOĞRAF
            "https://i.hizliresim.com/8f2ifh8.jpeg",                // 3. FOTOĞRAF
            "https://i.hizliresim.com/b83b2xf.jpeg",                // 4. FOTOĞRAF
            "https://i.hizliresim.com/2dfq21d.jpeg",                // 5. FOTOĞRAF
            "https://i.hizliresim.com/2sfqrbl.jpeg",                // 6. FOTOĞRAF
            "LİNK_7_BURAYA_YAPISTIR"                 // 7. FOTOĞRAF
        ];
        
        // Boş bırakılan veya geçersiz olan linkleri otomatik olarak atlar
        const FOTOGRAFLAR = HAM_FOTOGRAFLAR.filter(link => link && link.includes("http"));

        let aktifFotoIndex = 0;
        const fotoElement = document.getElementById('target-photo');

        // Resim linki kırık çıkarsa ekranda Hata Göster
        fotoElement.onerror = function() {
            this.src = "https://via.placeholder.com/400x400/000000/00ff00?text=FOTOGRAF+YUKLENEMEDI";
        };

        // --- TERMİNAL YAZI EFEKTİ (Açılış) ---
        let currentLine = 0;
        let currentChar = 0;
        const typewriterEl = document.getElementById('typewriter');
        
        function typeWriter() {
            const textLines = [
                "> system_boot --DOWN!",
                "> User: Binbasi GÖKHAN TOPRAK KOĞAN",
                "> user lvl: 19",
                "> derleniyor... [" + FOTOGRAFLAR.length + " DOSYA BULUNDU]",
                "> Uyarı: Bolum sonu canavari devrede.",
                "> Lütfen KIRMIZI BUTONA BAS KNK ."
            ];

            if (currentLine < textLines.length) {
                if (currentChar < textLines[currentLine].length) {
                    typewriterEl.innerHTML += textLines[currentLine].charAt(currentChar);
                    currentChar++;
                    setTimeout(typeWriter, Math.random() * 30 + 15); 
                } else {
                    typewriterEl.innerHTML += '<br>';
                    currentLine++;
                    currentChar = 0;
                    setTimeout(typeWriter, 300);
                }
            } else {
                typewriterEl.innerHTML += '<span class="cursor"></span>';
                document.getElementById('start-btn').style.display = 'block';
            }
        }

        window.onload = () => {
            setTimeout(typeWriter, 500);
        };

        // --- SES SİSTEMİ (Web Audio API) ---
        let audioCtx;
        function initAudio() {
            if (!audioCtx) {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            }
            if (audioCtx.state === 'suspended') audioCtx.resume();
        }

        function playBeep(freq, type, duration, vol=0.1) {
            if (!audioCtx) return;
            const osc = audioCtx.createOscillator();
            const gainNode = audioCtx.createGain();
            osc.type = type; 
            osc.frequency.setValueAtTime(freq, audioCtx.currentTime);
            gainNode.gain.setValueAtTime(vol, audioCtx.currentTime);
            gainNode.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + duration);
            osc.connect(gainNode);
            gainNode.connect(audioCtx.destination);
            osc.start();
            osc.stop(audioCtx.currentTime + duration);
        }

        let beepInterval;
        function startBeeping() {
            beepInterval = setInterval(() => {
                playBeep(880, 'square', 0.1, 0.08);
            }, 600); 
        }
        function stopBeeping() {
            clearInterval(beepInterval);
        }

        function playExplosion() {
            playBeep(100, 'sawtooth', 0.8, 0.4);
            setTimeout(() => playBeep(50, 'square', 1.2, 0.4), 100);
            setTimeout(() => playBeep(30, 'sawtooth', 1.5, 0.3), 300);
        }

        function playHappyBirthday() {
            const notes = [
                {f: 392, d: 300}, {f: 392, d: 300}, {f: 440, d: 600}, {f: 392, d: 600}, {f: 523, d: 600}, {f: 494, d: 1200},
                {f: 392, d: 300}, {f: 392, d: 300}, {f: 440, d: 600}, {f: 392, d: 600}, {f: 587, d: 600}, {f: 523, d: 1200},
                {f: 392, d: 300}, {f: 392, d: 300}, {f: 784, d: 600}, {f: 659, d: 600}, {f: 523, d: 600}, {f: 494, d: 600}, {f: 440, d: 900},
                {f: 698, d: 300}, {f: 698, d: 300}, {f: 659, d: 600}, {f: 523, d: 600}, {f: 587, d: 600}, {f: 523, d: 1500}
            ];
            
            let time = audioCtx.currentTime;
            notes.forEach(note => {
                const osc = audioCtx.createOscillator();
                const gainNode = audioCtx.createGain();
                osc.type = 'square';
                osc.frequency.value = note.f;
                gainNode.gain.setValueAtTime(0.08, time); 
                gainNode.gain.exponentialRampToValueAtTime(0.01, time + (note.d / 1000));
                osc.connect(gainNode);
                gainNode.connect(audioCtx.destination);
                osc.start(time);
                osc.stop(time + (note.d / 1000));
                time += (note.d / 1000) + 0.05; 
            });
        }

        // --- GÖRSEL SİSTEM (Canvas 2D) ---
        const canvas = document.getElementById('canvas');
        const ctx = canvas.getContext('2d');
        const startBtn = document.getElementById('start-btn');
        const flashEl = document.getElementById('flash');

        let width, height, cx, cy;
        let state = 0; 
        let particles = [];
        let towers = [];
        let plane = { x: -50, y: 0, speed: 2.0, crashed: false };

        function resize() {
            width = canvas.width = window.innerWidth;
            height = canvas.height = window.innerHeight;
            cx = width / 2;
            cy = height / 2;
            plane.y = cy; 
        }
        window.addEventListener('resize', resize);
        resize();

        function initTowers() {
            const towerW = Math.min(40, width * 0.1); 
            const towerH = height * 0.6; 
            const startX = width - (towerW * 4); 
            
            towers = [
                { x: startX, y: height - towerH, w: towerW, h: towerH },
                { x: startX + (towerW * 1.5), y: height - towerH, w: towerW, h: towerH },
                { x: startX + (towerW * 3), y: height - towerH, w: towerW, h: towerH }
            ];
        }

        class ExplosionParticle {
            constructor(x, y) {
                this.x = x;
                this.y = y;
                const angle = Math.random() * Math.PI * 2;
                const speed = Math.random() * 12 + 2; 
                this.vx = Math.cos(angle) * speed;
                this.vy = Math.sin(angle) * speed;
                this.size = Math.random() * 4 + 2; 
                this.alpha = 1;
                this.decay = Math.random() * 0.02 + 0.01;
            }
            update() {
                this.vx *= 0.95;
                this.vy *= 0.95;
                this.x += this.vx;
                this.y += this.vy;
                this.alpha -= this.decay;
            }
            draw() {
                if (this.alpha <= 0) return;
                ctx.fillStyle = `rgba(0, 255, 0, ${this.alpha})`;
                ctx.fillRect(this.x, this.y, this.size, this.size);
            }
        }

        startBtn.addEventListener('click', () => {
            initAudio();
            startBtn.style.display = 'none';
            document.getElementById('terminal-ui').style.display = 'none'; 
            
            initTowers();
            plane.x = -50;
            plane.crashed = false;
            
            startBeeping(); 
            state = 1; 
        });

        function triggerFlash() {
            flashEl.style.opacity = '1';
            flashEl.style.transition = 'none';
            setTimeout(() => {
                flashEl.style.transition = 'opacity 1s ease-out';
                flashEl.style.opacity = '0';
            }, 100);
        }

        // --- MÜKEMMEL FOTOĞRAF DÖNGÜSÜ ---
        function startPhotoSlideshow() {
            if(FOTOGRAFLAR.length > 0) {
                fotoElement.src = FOTOGRAFLAR[aktifFotoIndex];
                fotoElement.style.display = 'block';
                
                if (FOTOGRAFLAR.length > 1) {
                    setInterval(() => {
                        // Resim değişmeden önce glitch efekti ver
                        fotoElement.classList.add('glitch-transition');
                        
                        setTimeout(() => {
                            aktifFotoIndex = (aktifFotoIndex + 1) % FOTOGRAFLAR.length;
                            fotoElement.src = FOTOGRAFLAR[aktifFotoIndex];
                            
                            // Animasyon bitince class'ı kaldır
                            setTimeout(() => {
                                fotoElement.classList.remove('glitch-transition');
                            }, 200);

                        }, 100); // 100ms sonra resmi değiştir

                    }, 3000); // Her fotoğraf ekranda 3 saniye kalır
                }
            }
        }

        function showFinalMessage() {
            document.getElementById('final-message-container').style.display = 'block';
            const msgEl = document.getElementById('final-message');
            const targetText = "İYİKİ DOĞDUN KARDİŞİMMMMMMM!!";
            let i = 0;
            
            function typeFinal() {
                if (i < targetText.length) {
                    msgEl.innerHTML = targetText.substring(0, i + 1) + '<span class="cursor"></span>';
                    i++;
                    setTimeout(typeFinal, 80); 
                } else {
                    setTimeout(() => {
                        startPhotoSlideshow();
                        showCredit();
                    }, 500);
                }
            }
            typeFinal();
        }

        function showCredit() {
            const creditEl = document.getElementById('credit');
            creditEl.style.display = 'block';
            const creditText = "> Designer by Yusuf Karabulut";
            let j = 0;
            
            function typeCredit() {
                if (j < creditText.length) {
                    creditEl.innerHTML = creditText.substring(0, j + 1) + '<span class="cursor"></span>';
                    j++;
                    setTimeout(typeCredit, 50);
                }
            }
            typeCredit();
        }

        let explosionTimer = 0;

        function animate() {
            ctx.fillStyle = '#030303';
            ctx.fillRect(0, 0, width, height);

            if (state === 1) {
                towers.forEach(t => {
                    ctx.strokeStyle = '#00aa00';
                    ctx.lineWidth = 2;
                    ctx.strokeRect(t.x, t.y, t.w, t.h);
                    ctx.beginPath();
                    for(let i=0; i<t.h; i+=15) {
                        ctx.moveTo(t.x, t.y + i);
                        ctx.lineTo(t.x + t.w, t.y + i);
                    }
                    ctx.stroke();
                });

                plane.x += plane.speed;
                ctx.fillStyle = '#ffffff';
                ctx.beginPath();
                ctx.moveTo(plane.x, plane.y);
                ctx.lineTo(plane.x - 30, plane.y - 12);
                ctx.lineTo(plane.x - 20, plane.y); 
                ctx.lineTo(plane.x - 30, plane.y + 12);
                ctx.fill();

                if (plane.x >= towers[0].x) {
                    stopBeeping(); 
                    playExplosion();
                    triggerFlash(); 
                    plane.crashed = true;
                    state = 2;
                    
                    for (let i = 0; i < 200; i++) {
                        particles.push(new ExplosionParticle(towers[0].x + 20, plane.y));
                    }
                }
            } 
            else if (state === 2) {
                let allDead = true;
                particles.forEach(p => { 
                    p.update(); 
                    p.draw(); 
                    if(p.alpha > 0) allDead = false;
                });
                
                explosionTimer++;
                if(explosionTimer < 100) {
                    ctx.strokeStyle = `rgba(0, 255, 0, ${(100-explosionTimer)/100})`;
                    ctx.lineWidth = 5;
                    ctx.beginPath();
                    ctx.arc(towers[0].x + 20, plane.y, explosionTimer * 8, 0, Math.PI*2);
                    ctx.stroke();
                }

                if (allDead && explosionTimer > 150) {
                    state = 3;
                    setTimeout(() => {
                        playHappyBirthday();
                        showFinalMessage();
                    }, 500);
                }
            }

            requestAnimationFrame(animate);
        }

        animate();

    </script>
</body>
</html>
