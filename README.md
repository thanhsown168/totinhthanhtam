<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Anh Yêu Em</title>

  <!-- Montserrat ExtraBold -->
  <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@800;900&display=swap" rel="stylesheet">

  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body {
      background: #000;
      overflow: hidden;
      height: 100vh;
      font-family: 'Montserrat', sans-serif;
      color: #fff;
      position: relative;
    }

    /* ====================== LOADING SCREEN ====================== */
    #loading {
      position: fixed;
      top: 0; left: 0;
      width: 100%; height: 100%;
      background: #000;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      z-index: 9999;
      transition: opacity 1s ease-out;
    }

    .spinner {
      width: 60px; height: 60px;
      border: 6px solid rgba(180, 120, 255, 0.3);
      border-top: 6px solid #b478ff;
      border-radius: 50%;
      animation: spin 1s linear infinite;
      margin-bottom: 30px;
    }

    @keyframes spin { to { transform: rotate(360deg); } }

    .loading-text {
      font-size: 36px;
      font-weight: 900;
      color: #b478ff;
      text-shadow: 0 0 15px #b478ff;
      animation: glitch 2s infinite;
    }

    .subtitle {
      margin-top: 20px;
      font-size: 20px;
      color: #aaa;
      animation: fade 2s infinite;
    }

    @keyframes glitch {
      0%, 100% { text-shadow: 2px 0 #b478ff, -2px 0 #0ff; }
      50% { text-shadow: -2px 0 #0ff, 2px 0 #b478ff; }
    }

    @keyframes fade {
      0%, 100% { opacity: 0.6; }
      50% { opacity: 1; }
    }

    /* ====================== MATRIX RAIN ====================== */
    canvas { position: absolute; top: 0; left: 0; z-index: 1; }

    /* ====================== ĐẾM NGƯỢC ====================== */
    #countdown {
      position: absolute;
      font-size: 130px;
      font-weight: 900;
      color: #b478ff;
      text-shadow: 0 0 30px #b478ff, 0 0 60px #9f5cff;
      z-index: 10;
      opacity: 0;
      top: 50%; left: 50%;
      transform: translate(-50%, -50%);
      animation: pulse 0.8s infinite;
    }

    @keyframes pulse {
      0%, 100% { transform: translate(-50%, -50%) scale(1); }
      50% { transform: translate(-50%, -50%) scale(1.12); }
    }

    /* ====================== 3 CÂU ====================== */
    .message {
      position: absolute;
      font-size: 52px;
      font-weight: 900;
      color: #fff;
      text-align: center;
      text-shadow: 
        0 0 15px #b478ff,
        0 0 30px #9f5cff,
        0 0 50px #b478ff;
      opacity: 0;
      z-index: 5;
      top: 50%; left: 50%;
      transform: translate(-50%, -50%);
      white-space: nowrap;
      pointer-events: none;
    }

    /* TRÁI TIM ĐỎ BỰ TRONG CÂU 3 */
    .big-heart-icon {
      display: inline-block;
      font-size: 80px;
      margin-left: 12px;
      color: #ff1493;
      animation: heartBeat 1.5s infinite;
      text-shadow: 0 0 15px #ff1493;
    }

    @keyframes heartBeat {
      0%, 100% { transform: scale(1); }
      50% { transform: scale(1.35); }
    }

    .explode {
      animation: explode 2.8s forwards;
    }

    @keyframes explode {
      0% { opacity: 0; transform: translate(-50%, -50%) scale(0.3); filter: blur(3px); }
      15% { opacity: 1; transform: translate(-50%, -50%) scale(1.15); filter: blur(0); }
      25% { opacity: 1; transform: translate(-50%, -50%) scale(1); }
      70% { opacity: 1; transform: translate(-50%, -50%) scale(1); }
      100% { opacity: 0; transform: translate(-50%, -50%) scale(2); filter: blur(8px); }
    }

    /* ====================== TRÁI TIM + CÂU CUỐI (TO HƠN, NẰM TRONG TIM) ====================== */
    #final {
      position: absolute;
      width: 400px; height: 400px;
      top: 50%; left: 50%;
      transform: translate(-50%, -50%);
      opacity: 0;
      z-index: 15;
      animation: finalFade 2s forwards 1s;
    }

    @keyframes finalFade { to { opacity: 1; } }

    .big-heart {
      position: relative;
      width: 140px; height: 126px;
      margin: 85px auto;
      transform: rotate(-45deg);
      animation: bigPulse 1.8s infinite;
    }

    .big-heart::before,
    .big-heart::after {
      content: '';
      width: 72px; height: 112px;
      position: absolute;
      background: #ff1493;
      border-radius: 70px 70px 0 0;
      box-shadow: 0 0 40px #ff1493, 0 0 80px #ff69b4;
    }

    .big-heart::before {
      left: 70px;
      transform: rotate(-45deg);
      transform-origin: 0 100%;
    }

    .big-heart::after {
      left: 0;
      transform: rotate(45deg);
      transform-origin: 100% 100%;
    }

    @keyframes bigPulse {
      0%, 100% { transform: rotate(-45deg) scale(1); }
      50% { transform: rotate(-45deg) scale(1.22); }
    }

    .final-text {
      position: absolute;
      top: 50%; left: 50%;
      transform: translate(-50%, -50%);
      color: white;
      font-size: 20px; /* TO HƠN */
      font-weight: 900;
      text-align: center;
      text-shadow: 0 0 20px #000;
      width: 300px;
      line-height: 1.5;
      animation: textGlow 2s infinite alternate;
    }

    @keyframes textGlow {
      from { text-shadow: 0 0 15px #fff, 0 0 30px #ff1493; }
      to { text-shadow: 0 0 25px #fff, 0 0 50px #ff69b4; }
    }

    /* ====================== RESPONSIVE ====================== */
    @media (max-width: 480px) {
      #countdown { font-size: 90px; }
      .message { font-size: 38px; }
      .big-heart-icon { font-size: 40px; }
      #final { width: 300px; height: 300px; }
      .big-heart { width: 110px; height: 99px; margin: 65px auto; }
      .big-heart::before, .big-heart::after { width: 56px; height: 86px; left: 54px; }
      .final-text { font-size: 24px; width: 240px; }
      .loading-text { font-size: 28px; }
      .subtitle { font-size: 16px; }
    }
  </style>
</head>
<body>

  <!-- LOADING -->
  <div id="loading">
    <div class="spinner"></div>
    <div class="loading-text">ĐANG TẢI...</div>
    <div class="subtitle">CHỈ CHỜ HẠNH PHÚC</div>
  </div>

  <!-- MATRIX CANVAS -->
  <canvas id="matrix"></canvas>

  <!-- ĐẾM NGƯỢC -->
  <div id="countdown">3</div>

  <!-- 3 CÂU -->
  <div id="msg1" class="message" style="display:none;">EM ĐỒNG Ý</div>
  <div id="msg2" class="message" style="display:none;">LÀM NGƯỜI YÊU</div>
  <div id="msg3" class="message" style="display:none;">
    ANH NHÉ <span class="big-heart-icon">♥</span>
  </div>

  <!-- TRÁI TIM + CÂU CUỐI -->
  <div id="final" style="display:none;">
    <div class="big-heart"></div>
    <div class="final-text">
      CẢM ƠN EM<br>ĐÃ ĐẾN BÊN ANH
    </div>
  </div>

  <script>
    // ====================== MATRIX RAIN: ANH YÊU EM (TÍM NHẠT) ======================
    const canvas = document.getElementById('matrix');
    const ctx = canvas.getContext('2d');
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    const text = 'ANH YÊU EM'.split('');
    const fontSize = 16;
    const columns = canvas.width / fontSize;
    const drops = Array(Math.floor(columns)).fill(1);

    function drawMatrix() {
      ctx.fillStyle = 'rgba(0, 0, 0, 0.04)';
      ctx.fillRect(0, 0, canvas.width, canvas.height);
      ctx.fillStyle = '#b478ff';
      ctx.font = `900 ${fontSize}px Montserrat`;

      for (let i = 0; i < drops.length; i++) {
        const char = text[Math.floor(Math.random() * text.length)];
        const x = i * fontSize;
        const y = drops[i] * fontSize;
        ctx.fillText(char, x, y);

        if (y > canvas.height && Math.random() > 0.975) drops[i] = 0;
        drops[i]++;
      }
    }

    const matrixInterval = setInterval(drawMatrix, 50);

    window.addEventListener('resize', () => {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
    });

    // ====================== LOADING → MAIN ======================
    window.addEventListener('load', () => {
      setTimeout(() => {
        document.getElementById('loading').style.opacity = '0';
        setTimeout(() => {
          document.getElementById('loading').style.display = 'none';
          startCountdown();
        }, 1000);
      }, 2500);
    });

    // ====================== ĐẾM NGƯỢC + 3 CÂU ======================
    function startCountdown() {
      const cd = document.getElementById('countdown');
      let count = 3;
      cd.style.opacity = '1';

      const timer = setInterval(() => {
        count--;
        if (count > 0) {
          cd.textContent = count;
        } else {
          clearInterval(timer);
          cd.style.opacity = '0';
          setTimeout(showMessages, 800);
        }
      }, 1000);
    }

    function showMessages() {
      const msgs = ['msg1', 'msg2', 'msg3'];
      let i = 0;

      function next() {
        if (i < msgs.length) {
          const el = document.getElementById(msgs[i]);
          el.style.display = 'block';
          setTimeout(() => el.classList.add('explode'), 100);
          setTimeout(() => { i++; next(); }, 3000);
        } else {
          setTimeout(showFinal, 1200);
        }
      }
      next();
    }

    function showFinal() {
      document.getElementById('final').style.display = 'block';
    }
  </script>
</body>
</html>
