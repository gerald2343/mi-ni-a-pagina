<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <title>Para mi Lunita</title>
  <style>
    body {
      background: black;
      margin: 0;
      overflow: hidden;
      height: 100vh;
      font-family: 'Segoe UI', sans-serif;
      position: relative;
      cursor: pointer;
    }

    .start-screen {
      position: absolute;
      width: 100%;
      height: 100%;
      background: black;
      color: white;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 24px;
      z-index: 20;
      text-align: center;
    }

    .center-te-amo {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      font-size: 60px;
      color: #ff4dd2;
      text-shadow: 0 0 20px #ff4dd2, 0 0 40px #ff4dd2;
      z-index: 10;
      display: none;
      animation: heartbeat 2s infinite;
      white-space: nowrap;
    }

    @keyframes heartbeat {
      0%, 100% { transform: translate(-50%, -50%) scale(1); }
      50% { transform: translate(-50%, -50%) scale(1.1); }
    }

    .rain-container {
      position: absolute;
      width: 100%;
      height: 100%;
      overflow: hidden;
      display: none;
    }

    .drop {
      position: absolute;
      top: -50px;
      font-size: 24px;
      font-weight: bold;
      opacity: 0;
      animation: fall linear forwards;
    }

    @keyframes fall {
      0% { transform: translateY(0); opacity: 0; }
      10% { opacity: 1; text-shadow: 0 0 10px #ff4dd2, 0 0 20px #ff1a75; }
      90% { opacity: 1; }
      100% { transform: translateY(100vh); opacity: 0; }
    }

    .tap-te-amo {
      position: absolute;
      font-size: 20px;
      font-weight: bold;
      pointer-events: none;
      opacity: 1;
      animation: fadeOut 2s ease-out forwards;
      white-space: nowrap;
    }

    @keyframes fadeOut {
      0% { transform: scale(1); opacity: 1; }
      100% { transform: scale(1.5); opacity: 0; }
    }
  </style>
</head>
<body>

  <div class="start-screen" id="startScreen">Toca para comenzar 💖</div>
  <div class="center-te-amo" id="mainText">Te amo</div>
  <div class="rain-container" id="rain"></div>

  <audio id="music" preload="auto" loop>
    <source src="zzz.mp3" type="audio/mpeg">
    Tu navegador no soporta audio.
  </audio>

  <script>
    const startScreen = document.getElementById('startScreen');
    const mainText = document.getElementById('mainText');
    const rain = document.getElementById('rain');
    const music = document.getElementById('music');
    const colors = ['#ff1a75', '#ff4dd2', '#ff0066', '#ff3399', '#ff66cc', '#ff1493'];

    function showTapTeAmo(x, y) {
      const text = document.createElement('div');
      text.classList.add('tap-te-amo');
      text.innerText = 'Te amo';
      text.style.left = `${x}px`;
      text.style.top = `${y}px`;
      text.style.color = colors[Math.floor(Math.random() * colors.length)];
      text.style.textShadow = `0 0 10px ${text.style.color}`;
      document.body.appendChild(text);
      setTimeout(() => text.remove(), 2000);
    }

    function startLoveScene(e) {
      startScreen.style.display = 'none';
      mainText.style.display = 'block';
      rain.style.display = 'block';

      // Reproduce música dentro de evento de interacción
      music.play().catch((err) => {
        console.warn("Error reproduciendo música:", err);
      });

      // Inicia lluvia
      setInterval(() => {
        const drop = document.createElement('div');
        drop.classList.add('drop');
        drop.innerText = 'Te amo';
        drop.style.left = Math.random() * window.innerWidth + 'px';
        drop.style.color = colors[Math.floor(Math.random() * colors.length)];
        drop.style.animationDuration = (3 + Math.random() * 2) + 's';
        rain.appendChild(drop);
        setTimeout(() => drop.remove(), 5000);
      }, 150);

      // Mostrar "te amo" donde tocó
      if (e.touches && e.touches.length > 0) {
        showTapTeAmo(e.touches[0].clientX, e.touches[0].clientY);
      } else {
        showTapTeAmo(e.clientX, e.clientY);
      }
    }

    // Al primer toque/clic
    startScreen.addEventListener('click', startLoveScene);
    startScreen.addEventListener('touchstart', startLoveScene);

    // Mostrar "te amo" en cada toque posterior
    document.body.addEventListener('click', (e) => {
      if (startScreen.style.display === 'none') {
        showTapTeAmo(e.clientX, e.clientY);
      }
    });

    document.body.addEventListener('touchstart', (e) => {
      if (startScreen.style.display === 'none') {
        const touch = e.touches[0];
        showTapTeAmo(touch.clientX, touch.clientY);
      }
    });
  </script>
</body>
</html>
