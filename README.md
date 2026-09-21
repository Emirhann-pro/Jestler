# Jestler<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Seni Seviyorum | CEYLİN & Emir</title>
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@600;800&family=Great+Vibes&family=Montserrat:wght@300;400;600&display=swap" rel="stylesheet">

    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: #09010f;
            background: radial-gradient(circle at center, #1e0324 0%, #08000d 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
            font-family: 'Montserrat', sans-serif;
            color: #ffffff;
            position: relative;
        }

        /* Başlangıç Overlay (Müzik Engeline Takılmamak İçin) */
        #startOverlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background: rgba(8, 0, 13, 0.95);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 100;
            cursor: pointer;
            transition: opacity 0.8s ease, visibility 0.8s ease;
        }

        #startOverlay h2 {
            font-family: 'Great Vibes', cursive;
            font-size: 3rem;
            color: #ff2a6d;
            text-shadow: 0 0 15px #ff0055;
            margin-bottom: 20px;
        }

        #startOverlay button {
            background: transparent;
            border: 2px solid #ff2a6d;
            color: #fff;
            padding: 12px 30px;
            font-size: 1rem;
            border-radius: 30px;
            cursor: pointer;
            box-shadow: 0 0 15px #ff2a6d;
            transition: 0.3s;
            font-family: 'Montserrat', sans-serif;
            letter-spacing: 2px;
        }

        #startOverlay button:hover {
            background: #ff2a6d;
            box-shadow: 0 0 30px #ff0055;
        }

        /* Canvas Arka Plan */
        #bgCanvas {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
            pointer-events: none;
        }

        /* Ana Sahne */
        .stage {
            position: relative;
            z-index: 2;
            width: 100vw;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        /* Kalp Kapsayıcısı */
        .heart-wrapper {
            position: relative;
            width: 750px;
            height: 650px;
            display: flex;
            justify-content: center;
            align-items: center;
            animation: pulse 2.5s infinite ease-in-out;
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.03); }
            100% { transform: scale(1); }
        }

        /* Kalbi Oluşturan Küçük Yazılar */
        .heart-word {
            position: absolute;
            font-size: 10px;
            font-weight: 600;
            color: #ff2a6d;
            text-shadow: 0 0 6px #072dff, 0 0 12px #072dff;
            white-space: nowrap;
            user-select: none;
            opacity: 0;
            transform: translate(-50%, -50%) scale(0);
            transition: opacity 0.5s ease, transform 0.5s ease;
        }

        .heart-word.visible {
            opacity: 1;
        }

        .heart-word:hover {
            transform: translate(-50%, -50%) scale(1.6) !important;
            color: #ffffff;
            text-shadow: 0 0 15px #ffffff, 0 0 25px #ff0055;
            z-index: 100;
        }

        /* Merkezdeki NAZ Yazısı */
        .center-box {
            position: absolute;
            top: 42%;
            left: 50%;
            transform: translate(-50%, -50%);
            text-align: center;
            z-index: 10;
            pointer-events: none;
            opacity: 0;
            transition: opacity 1.5s ease;
        }

        .center-box.show {
            opacity: 1;
        }

        .center-box h1 {
            font-family: 'Cinzel', serif;
            font-size: 4.5rem;
            font-weight: 800;
            color: #ffffff;
            letter-spacing: 8px;
            text-shadow: 
                0 0 10px #1b3cf7,
                0 0 20px #1b3cf7,
                0 0 40px #1b3cf7,
                0 0 70px #1b3cf7;
        }

        .center-box p {
            font-family: 'Great Vibes', cursive;
            font-size: 1.8rem;
            color: #ffa1b8;
            text-shadow: 0 0 10px #1b3cf7;
        }

        /* Alt Mesaj Kartı */
        .quote-card {
            position: absolute;
            bottom: 10%;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(20, 3, 25, 0.75);
            border: 1px solid rgba(255, 42, 109, 0.4);
            border-radius: 20px;
            padding: 18px 35px;
            backdrop-filter: blur(10px);
            box-shadow: 0 0 25px rgba(255, 0, 85, 0.35);
            text-align: center;
            z-index: 10;
            opacity: 0;
            transition: opacity 1.5s ease 0.5s;
            max-width: 85%;
        }

        .quote-card.show {
            opacity: 1;
        }

        .quote-card .main-quote {
            font-size: 1.05rem;
            font-style: italic;
            font-weight: 300;
            color: #ffffff;
            margin-bottom: 6px;
            letter-spacing: 0.5px;
        }

        .quote-card .sub-quote {
            font-size: 0.85rem;
            color: #ff7597;
            font-weight: 600;
            letter-spacing: 1.5px;
        }

        @media (max-width: 768px) {
            .heart-wrapper {
                width: 90vw;
                height: 480px;
            }
            .center-box h1 {
                font-size: 3rem;
            }
            .center-box p {
                font-size: 1.3rem;
            }
            .quote-card .main-quote {
                font-size: 0.85rem;
            }
        }
    </style>
</head>
<body>

    <!-- Başlatma Ekranı (Tarayıcı Müzik Engeli İçin) -->
    <div id="startOverlay" onclick="startSurprise()">
        <h2>Aşkın İçin Tıkla...</h2>
        <button>SÜRPRİZİ BAŞLAT</button>
    </div>

    <!-- Müzik (Angel - Massi Havasında Duygusal Telifsiz Piyano) -->
    <audio id="bgMusic" loop preload="auto">
        <source src="https://cdn.pixabay.com/download/audio/2022/05/27/audio_1808fbf07a.mp3?filename=sad-piano-112282.mp3" type="audio/mpeg">
    </audio>

    <!-- Parıldayan Arka Plan Canvas -->
    <canvas id="bgCanvas"></canvas>

    <div class="stage">
        <div class="heart-wrapper" id="heartWrapper">
            
            <!-- Ortadaki İsim -->
            <div class="center-box" id="centerBox">
                <h1>CEYLİN</h1>
                <p>Ömrümün En Güzel Aşkı</p>
            </div>

            <!-- Alt Mesaj Kutusu -->
            <div class="quote-card" id="quoteCard">
                <div class="main-quote">"Seninle geçen her an, ömrümün en kıymetli hazinesidir."</div>
                <div class="sub-quote">✨ Sevgilime Özel (Seni Her Şeyden Çok Seviyorumm) ✨</div>
            </div>

        </div>
    </div>

    <script>
        // 1. SÜRPRİZİ BAŞLATMA FONKSİYONU
        function startSurprise() {
            const overlay = document.getElementById('startOverlay');
            const music = document.getElementById('bgMusic');
            
            overlay.style.opacity = '0';
            setTimeout(() => {
                overlay.style.visibility = 'hidden';
            }, 800);

            // Müziği oynat
            music.play().catch(e => console.log("Müzik başlatılamadı:", e));

            // Kalbi çizmeye başla
            buildHeart();
        }

        // 2. KALP MATEMATİK DENKLEMİ VE KELİMELERİN YAZILMASI
        const heartWrapper = document.getElementById('heartWrapper');
        const centerBox = document.getElementById('centerBox');
        const quoteCard = document.getElementById('quoteCard');

        const words = [
            "LOVE YOU", "SENİ SEVİYORUM", "CEYLİN ♥ EMİR", "FOREVER", 
            "MY ANGEL", "İYİ Kİ VARSIN", "Sonsuzum", "AŞKIM", 
            "LOVE", "HER ŞEYİM", "BİTANEM", "C ♥ E", "I LOVE YOU", 
            "SOL YANIM", "BİR TANEM", "I MISS YOU", "MELEĞİM"
        ];

        function getHeartPoint(t, scale) {
            const x = scale * 16 * Math.pow(Math.sin(t), 3);
            const y = -scale * (13 * Math.cos(t) - 5 * Math.cos(2*t) - 2 * Math.cos(3*t) - Math.cos(4*t));
            return { x, y };
        }

        function buildHeart() {
            const pointsData = [];
            const totalPoints = 130;

            // Dış Katman
            for (let i = 0; i < totalPoints; i++) {
                const t = (i / totalPoints) * Math.PI * 2;
                pointsData.push({ pt: getHeartPoint(t, 13.5), scale: 13.5 });
            }
            // İç Katman
            for (let i = 0; i < totalPoints * 0.6; i++) {
                const t = (i / (totalPoints * 0.6)) * Math.PI * 2;
                pointsData.push({ pt: getHeartPoint(t, 9.5), scale: 9.5 });
            }
            // En İç Katman
            for (let i = 0; i < totalPoints * 0.3; i++) {
                const t = (i / (totalPoints * 0.3)) * Math.PI * 2;
                pointsData.push({ pt: getHeartPoint(t, 5.5), scale: 5.5 });
            }

            // Sırayla ekrana çizdirme animasyonu
            pointsData.forEach((item, index) => {
                setTimeout(() => {
                    createWordSpan(item.pt.x, item.pt.y);
                }, index * 25); // 25ms aralıklarla tek tek yazılır
            });

            // Kalp bittiğinde yazıları ve alt kutuyu göster
            setTimeout(() => {
                centerBox.classList.add('show');
                quoteCard.classList.add('show');
            }, pointsData.length * 25 + 200);
        }

        function createWordSpan(x, y) {
            const span = document.createElement('span');
            span.className = 'heart-word';
            
            const randomWord = words[Math.floor(Math.random() * words.length)];
            span.innerText = randomWord;

            const angle = Math.atan2(y, x) * (180 / Math.PI);
            
            span.style.left = `calc(50% + ${x}px)`;
            span.style.top = `calc(44% + ${y}px)`;
            
            const randomScale = (Math.random() * 0.3 + 0.75).toFixed(2);
            const initialTransform = `translate(-50%, -50%) rotate(${angle * 0.25}deg) scale(${randomScale})`;
            
            span.style.transform = initialTransform;

            const colors = ['#ff2a6d', '#ff0055', '#ff7597', '#ffffff', '#ff1493'];
            span.style.color = colors[Math.floor(Math.random() * colors.length)];

            heartWrapper.appendChild(span);

            // Animasyon açılışı
            setTimeout(() => {
                span.classList.add('visible');
            }, 10);
        }

        // 3. CANLI ARKA PLAN PARÇACIKLARI (CANVAS)
        const canvas = document.getElementById('bgCanvas');
        const ctx = canvas.getContext('2d');

        function resizeCanvas() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        }
        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();

        const particles = [];
        for (let i = 0; i < 60; i++) {
            particles.push({
                x: Math.random() * canvas.width,
                y: Math.random() * canvas.height,
                size: Math.random() * 2 + 1,
                speedX: (Math.random() - 0.5) * 0.4,
                speedY: (Math.random() - 0.5) * 0.4,
                opacity: Math.random()
            });
        }

        function animateBg() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            particles.forEach(p => {
                p.x += p.speedX;
                p.y += p.speedY;
                if (p.x < 0 || p.x > canvas.width) p.speedX *= -1;
                if (p.y < 0 || p.y > canvas.height) p.speedY *= -1;

                ctx.beginPath();
                ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
                ctx.fillStyle = `rgba(255, 42, 109, ${p.opacity})`;
                ctx.shadowBlur = 8;
                ctx.shadowColor = '#ff0055';
                ctx.fill();
            });
            requestAnimationFrame(animateBg);
        }
        animateBg();
    </script>
</body>
</html>
