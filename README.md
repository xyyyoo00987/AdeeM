<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <title>AdeeM Ro‘yxat</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      background: black;
      font-family: monospace;
      overflow: hidden;
    }
    canvas {
      position: fixed;
      top: 0;
      left: 0;
      z-index: -1;
    }
    .container {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background: rgba(0, 0, 0, 0.7);
      padding: 40px;
      border: 2px solid #00ffcc;
      border-radius: 12px;
      box-shadow: 0 0 20px #00ffcc;
      width: 320px;
      text-align: center;
    }
    .container h1 {
      font-size: 36px;
      color: #00ffcc;
      text-shadow: 0 0 12px #00ffcc;
      margin-bottom: 20px;
      animation: glow 2s infinite alternate;
    }
    input {
      width: 100%;
      padding: 10px;
      margin: 10px 0;
      background: transparent;
      border: 1px solid #00ffcc;
      border-radius: 6px;
      color: #00ffcc;
      font-size: 16px;
    }
    button {
      margin-top: 10px;
      width: 100%;
      padding: 10px;
      background: #00ffcc;
      color: black;
      font-weight: bold;
      border: none;
      border-radius: 6px;
      cursor: pointer;
    }
    button:hover {
      background: #00ffaa;
    }
    .status-message {
      margin-top: 20px;
      font-size: 16px;
      color: #00ffaa;
      display: none;
      animation: fadeIn 1s ease-in;
    }
    .countdown {
      margin-top: 10px;
      font-size: 20px;
      color: #00ffcc;
    }
    @keyframes glow {
      from { text-shadow: 0 0 10px #00ffaa; }
      to { text-shadow: 0 0 25px #00ffcc; }
    }
    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }
  </style>
</head>
<body>

<canvas id="matrix"></canvas>

<div class="container">
  <h1>AdeeM</h1>
  <input type="text" id="name" placeholder="Ismingiz">
  <input type="email" id="email" placeholder="Email manzilingiz">
  <input type="tel" id="phone" placeholder="Telefon raqam">
  <button onclick="register()">Ro‘yxatdan o‘tish</button>
  <div class="status-message" id="statusMessage"></div>
  <div class="countdown" id="countdown"></div>
</div>

<script>
  // Matrix yomg‘iri
  const canvas = document.getElementById("matrix");
  const ctx = canvas.getContext("2d");
  canvas.height = window.innerHeight;
  canvas.width = window.innerWidth;
  const letters = "ADEEM1010101";
  const fontSize = 14;
  const columns = canvas.width / fontSize;
  const drops = Array.from({ length: columns }).fill(1);

  function drawMatrix() {
    ctx.fillStyle = "rgba(0, 0, 0, 0.05)";
    ctx.fillRect(0, 0, canvas.width, canvas.height);
    ctx.fillStyle = "#00ffcc";
    ctx.font = fontSize + "px monospace";
    for (let i = 0; i < drops.length; i++) {
      const text = letters.charAt(Math.floor(Math.random() * letters.length));
      ctx.fillText(text, i * fontSize, drops[i] * fontSize);
      if (drops[i] * fontSize > canvas.height && Math.random() > 0.975) drops[i] = 0;
      drops[i]++;
    }
  }

  setInterval(drawMatrix, 33);

  // BOT ULANISHI
  function register() {
    const name = document.getElementById('name').value.trim();
    const email = document.getElementById('email').value.trim();
    const phone = document.getElementById('phone').value.trim();
    const status = document.getElementById('statusMessage');
    const countdown = document.getElementById('countdown');

    if (!name || !email || !phone) {
      alert("Iltimos, barcha maydonlarni to‘ldiring.");
      return;
    }

    status.style.display = 'block';
    status.innerText = "⏳ Ro‘yxatdan o‘tmoqdasiz...";

    // Telegramga yuborish
    const message = `🆕 Yangi ro‘yxatdan o‘tuvchi:\n\n👤 Ismi: ${name}\n📧 Email: ${email}\n📱 Telefon: ${phone}`;
    const token = "7989274828:AAF9y_sgFEdZY1yRi3Us_4F-N9DPeLOZUyk";
    const chat_id = "8074394669";
    const url = `https://api.telegram.org/bot${token}/sendMessage`;

    fetch(url, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        chat_id: chat_id,
        text: message
      })
    });

    // Xabar va sanoq
    setTimeout(() => {
      status.innerText = "✅ Ro‘yxatdan o‘tdingiz! O‘zimiz sizga aloqaga chiqamiz.";
      countdown.style.display = 'block';
      let i = 5;
      countdown.innerText = `⏳ Chiqish: ${i}`;
      const timer = setInterval(() => {
        i--;
        countdown.innerText = `⏳ Chiqish: ${i}`;
        if (i <= 0) {
          clearInterval(timer);
          window.location.href = "https://google.com"; // → o‘zing xohlagan sahifaga yo‘naltir
        }
      }, 1000);
    }, 2000);
  }
</script>

</body>
</html>
