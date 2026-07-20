<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Podium.gg — Premium DLC</title>

  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;500;700;900&display=swap" rel="stylesheet" />
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css" />

  <style>
    /* ---------- Глобальные сбросы ---------- */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: 'Inter', sans-serif;
    }

    body {
      background: #000;
      overflow-x: hidden;
      color: #fff;
    }

    canvas {
      position: fixed;
      inset: 0;
      z-index: -2;
    }

    #noise {
      position: fixed;
      inset: 0;
      pointer-events: none;
      opacity: 0.06;
      background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='160' height='160'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.8'/%3E%3C/filter%3E%3Crect width='160' height='160' filter='url(%23n)'/%3E%3C/svg%3E");
      mix-blend-mode: overlay;
      z-index: -1;
    }

    .cursorGlow {
      position: fixed;
      width: 500px;
      height: 500px;
      border-radius: 50%;
      pointer-events: none;
      background: radial-gradient(circle, rgba(255, 255, 255, 0.16), transparent 70%);
      transform: translate(-50%, -50%);
      z-index: -1;
    }

    /* ---------- Навигация ---------- */
    nav {
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      padding: 20px 10%;
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: rgba(0, 0, 0, 0.6);
      backdrop-filter: blur(12px);
      border-bottom: 1px solid rgba(255, 255, 255, 0.08);
      z-index: 100;
    }

    .logo {
      font-size: 24px;
      font-weight: 900;
      letter-spacing: 3px;
      background: linear-gradient(135deg, #fff, #aaa);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }

    .nav-links {
      display: flex;
      gap: 40px;
    }

    .nav-links a {
      color: rgba(255, 255, 255, 0.7);
      text-decoration: none;
      font-weight: 500;
      font-size: 16px;
      transition: 0.3s;
      position: relative;
    }

    .nav-links a::after {
      content: '';
      position: absolute;
      bottom: -4px;
      left: 0;
      width: 0;
      height: 2px;
      background: #fff;
      transition: 0.3s;
    }

    .nav-links a:hover {
      color: #fff;
    }

    .nav-links a:hover::after {
      width: 100%;
    }

    /* ---------- Хедер ---------- */
    header {
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      text-align: center;
      position: relative;
      padding-top: 80px;
    }

    h1 {
      font-size: 80px;
      font-weight: 900;
      letter-spacing: 8px;
      animation: glitch 5s infinite;
    }

    .subtitle {
      margin-top: 20px;
      opacity: 0.75;
      font-size: 20px;
    }

    .btn-primary {
      margin-top: 50px;
      padding: 20px 60px;
      font-size: 20px;
      border: none;
      cursor: pointer;
      background: #fff;
      color: #000;
      font-weight: 700;
      border-radius: 50px;
      transition: 0.3s;
    }

    .btn-primary:hover {
      transform: scale(1.1);
      box-shadow: 0 0 80px rgba(255, 255, 255, 0.7);
    }

    /* ---------- Секции ---------- */
    section {
      padding: 120px 10% 80px;
    }

    .section-title {
      font-size: 50px;
      font-weight: 900;
      margin-bottom: 50px;
      text-align: center;
    }

    /* ---------- Тарифы ---------- */
    .tariff-container {
      max-width: 900px;
      margin: 0 auto;
      background: rgba(255, 255, 255, 0.04);
      backdrop-filter: blur(16px);
      border: 1px solid rgba(255, 255, 255, 0.1);
      border-radius: 40px;
      padding: 50px 40px;
    }

    .days-selector {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 14px;
      margin-bottom: 40px;
    }

    .day-btn {
      padding: 14px 32px;
      border-radius: 60px;
      border: 1px solid rgba(255, 255, 255, 0.2);
      background: transparent;
      color: rgba(255, 255, 255, 0.7);
      font-weight: 700;
      font-size: 16px;
      cursor: pointer;
      transition: 0.3s;
    }

    .day-btn:hover {
      border-color: #fff;
      color: #fff;
      background: rgba(255, 255, 255, 0.05);
    }

    .day-btn.active {
      background: #fff;
      color: #000;
      border-color: #fff;
      box-shadow: 0 0 30px rgba(255, 255, 255, 0.3);
    }

    .price-display {
      text-align: center;
      font-size: 64px;
      font-weight: 900;
      margin: 30px 0 40px;
      letter-spacing: 2px;
    }

    .price-display small {
      font-size: 24px;
      font-weight: 300;
      opacity: 0.6;
    }

    /* Способ оплаты (только FunPay) */
    .payment-methods {
      display: flex;
      justify-content: center;
      margin-bottom: 40px;
    }

    .payment-method {
      display: flex;
      align-items: center;
      gap: 14px;
      padding: 18px 44px;
      border-radius: 60px;
      border: 1px solid rgba(255, 255, 255, 0.2);
      background: rgba(255, 255, 255, 0.05);
      color: #fff;
      font-weight: 700;
      font-size: 22px;
      cursor: default;
      transition: 0.3s;
      box-shadow: 0 0 30px rgba(255, 255, 255, 0.05);
    }

    .payment-method i {
      font-size: 36px;
      color: #ffb347;
    }

    .btn-buy {
      display: block;
      width: 100%;
      max-width: 320px;
      margin: 0 auto;
      padding: 18px 40px;
      font-size: 22px;
      font-weight: 700;
      border: none;
      border-radius: 60px;
      background: linear-gradient(135deg, #fff, #ccc);
      color: #000;
      cursor: pointer;
      transition: 0.3s;
    }

    .btn-buy:hover {
      transform: scale(1.04);
      box-shadow: 0 0 60px rgba(255, 255, 255, 0.3);
    }

    /* ---------- Поддержка ---------- */
    .support-wrapper {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 30px;
    }

    .support-wrapper p {
      font-size: 20px;
      opacity: 0.7;
      max-width: 500px;
      text-align: center;
    }

    .discord-link {
      display: inline-flex;
      align-items: center;
      gap: 16px;
      padding: 18px 44px;
      border-radius: 60px;
      background: #5865f2;
      color: #fff;
      font-size: 24px;
      font-weight: 700;
      text-decoration: none;
      transition: 0.3s;
      box-shadow: 0 0 40px rgba(88, 101, 242, 0.3);
    }

    .discord-link i {
      font-size: 40px;
    }

    .discord-link:hover {
      transform: scale(1.08);
      box-shadow: 0 0 80px rgba(88, 101, 242, 0.6);
    }

    /* ---------- Модальное окно ---------- */
    .modal-overlay {
      display: none;
      position: fixed;
      inset: 0;
      background: rgba(0, 0, 0, 0.7);
      backdrop-filter: blur(8px);
      justify-content: center;
      align-items: center;
      z-index: 999;
    }

    .modal-overlay.show {
      display: flex;
    }

    .modal {
      background: #1a1a1a;
      border: 1px solid rgba(255, 255, 255, 0.15);
      border-radius: 40px;
      padding: 50px 40px;
      max-width: 440px;
      width: 90%;
      text-align: center;
    }

    .modal h3 {
      font-size: 28px;
      margin-bottom: 20px;
    }

    .modal p {
      opacity: 0.7;
      margin-bottom: 30px;
    }

    .modal .btn-primary {
      margin-top: 0;
      padding: 14px 40px;
      font-size: 18px;
    }

    .modal .btn-secondary {
      background: transparent;
      border: 1px solid rgba(255, 255, 255, 0.2);
      color: #fff;
      padding: 14px 40px;
      border-radius: 60px;
      font-size: 18px;
      font-weight: 700;
      cursor: pointer;
      transition: 0.3s;
      margin-left: 12px;
    }

    .modal .btn-secondary:hover {
      border-color: #fff;
    }

    /* ---------- Футер ---------- */
    footer {
      padding: 80px 10%;
      text-align: center;
      opacity: 0.6;
      border-top: 1px solid rgba(255, 255, 255, 0.05);
      font-weight: 300;
      letter-spacing: 1px;
    }

    /* ---------- Анимации ---------- */
    @keyframes glitch {
      0%, 100% {
        text-shadow: 0 0 10px #fff, 0 0 30px #fff;
      }
      20% {
        transform: translateX(2px);
      }
      21% {
        transform: translateX(-2px);
      }
      40% {
        filter: blur(2px);
      }
      60% {
        transform: rotate(0.3deg);
      }
      80% {
        letter-spacing: 14px;
      }
    }

    .floating {
      position: absolute;
      border: 1px solid rgba(255, 255, 255, 0.12);
      border-radius: 50%;
      animation: float linear infinite;
      pointer-events: none;
    }

    @keyframes float {
      from {
        transform: translateY(120vh) rotate(0);
      }
      to {
        transform: translateY(-150vh) rotate(360deg);
      }
    }

    /* ---------- Адаптивность ---------- */
    @media (max-width: 768px) {
      nav {
        padding: 16px 6%;
        flex-wrap: wrap;
        gap: 12px;
      }
      .nav-links {
        gap: 20px;
      }
      h1 {
        font-size: 48px;
        letter-spacing: 4px;
      }
      .subtitle {
        font-size: 16px;
      }
      .btn-primary {
        padding: 16px 40px;
        font-size: 16px;
      }
      section {
        padding: 80px 6% 60px;
      }
      .section-title {
        font-size: 34px;
      }
      .tariff-container {
        padding: 30px 20px;
      }
      .price-display {
        font-size: 44px;
      }
      .day-btn {
        padding: 10px 20px;
        font-size: 14px;
      }
      .payment-method {
        padding: 14px 28px;
        font-size: 18px;
      }
      .payment-method i {
        font-size: 28px;
      }
      .discord-link {
        font-size: 18px;
        padding: 14px 30px;
      }
      .discord-link i {
        font-size: 30px;
      }
      footer {
        padding: 60px 6%;
        font-size: 14px;
      }
    }
  </style>
</head>
<body>

  <!-- Фон -->
  <canvas id="bg"></canvas>
  <div id="noise"></div>
  <div class="cursorGlow"></div>

  <!-- Навигация -->
  <nav>
    <div class="logo">PODIUM.GG</div>
    <div class="nav-links">
      <a href="#plans">Тарифы</a>
      <a href="#support">Поддержка</a>
    </div>
  </nav>

  <!-- Хедер -->
  <header>
    <h1>PODIUM.GG</h1>
    <div class="subtitle">PREMIUM DLC EXPERIENCE</div>
    <button class="btn-primary" onclick="document.getElementById('plans').scrollIntoView({behavior:'smooth'})">
      ПОСМОТРЕТЬ ТАРИФЫ
    </button>
  </header>

  <!-- Тарифы -->
  <section id="plans">
    <h2 class="section-title">Выберите тариф</h2>
    <div class="tariff-container">
      <!-- Выбор дней -->
      <div class="days-selector" id="daysSelector">
        <button class="day-btn" data-days="1" data-price="99">1 день</button>
        <button class="day-btn" data-days="3" data-price="159">3 дня</button>
        <button class="day-btn" data-days="7" data-price="229">7 дней</button>
        <button class="day-btn" data-days="14" data-price="349">14 дней</button>
        <button class="day-btn active" data-days="30" data-price="549">30 дней</button>
      </div>

      <!-- Цена -->
      <div class="price-display" id="priceDisplay">
        549 <small>₽</small>
      </div>

      <!-- Способ оплаты (только FunPay) -->
      <div class="payment-methods">
        <div class="payment-method">
          <i class="fas fa-coins"></i> FunPay
        </div>
      </div>

      <!-- Кнопка оплаты -->
      <button class="btn-buy" id="buyBtn">Оплатить</button>
    </div>
  </section>

  <!-- Поддержка -->
  <section id="support">
    <h2 class="section-title">Поддержка</h2>
    <div class="support-wrapper">
      <p>Присоединяйтесь к нашему Discord-серверу — там мы всегда на связи</p>
      <a href="https://discord.gg/sUNGPKadB" target="_blank" class="discord-link">
        <i class="fab fa-discord"></i> Discord
      </a>
    </div>
  </section>

  <!-- Футер (обновлён) -->
  <footer>
    © 2026 Podium.gg Все права защищены.
  </footer>

  <!-- Модальное окно -->
  <div class="modal-overlay" id="modalOverlay">
    <div class="modal">
      <h3>✅ Готово!</h3>
      <p>Вы выбрали <strong id="modalDays">30 дней</strong> за <strong id="modalPrice">549 ₽</strong>. Ссылка на оплату через <strong>FunPay</strong> будет отправлена в Telegram.</p>
      <div>
        <button class="btn-primary" onclick="closeModal()">Отлично</button>
        <button class="btn-secondary" onclick="closeModal()">Закрыть</button>
      </div>
    </div>
  </div>

  <script>
    // ===== Свечение за курсором =====
    const glow = document.querySelector('.cursorGlow');
    window.addEventListener('mousemove', e => {
      glow.style.left = e.clientX + 'px';
      glow.style.top = e.clientY + 'px';
    });

    // ===== Звёзды =====
    const c = document.getElementById('bg');
    const ctx = c.getContext('2d');

    function resize() {
      c.width = window.innerWidth;
      c.height = window.innerHeight;
    }
    resize();
    window.addEventListener('resize', resize);

    const stars = [];
    for (let i = 0; i < 250; i++) {
      stars.push({
        x: Math.random() * c.width,
        y: Math.random() * c.height,
        r: Math.random() * 2,
        v: Math.random() * 0.5 + 0.2
      });
    }

    function draw() {
      ctx.clearRect(0, 0, c.width, c.height);
      for (const s of stars) {
        ctx.beginPath();
        ctx.arc(s.x, s.y, s.r, 0, 7);
        ctx.fillStyle = '#fff';
        ctx.fill();
        s.y += s.v;
        if (s.y > c.height) {
          s.y = 0;
          s.x = Math.random() * c.width;
        }
      }
      requestAnimationFrame(draw);
    }
    draw();

    // ===== Плавающие круги =====
    for (let i = 0; i < 35; i++) {
      const d = document.createElement('div');
      d.className = 'floating';
      const s = Math.random() * 220 + 40;
      d.style.width = s + 'px';
      d.style.height = s + 'px';
      d.style.left = Math.random() * 100 + 'vw';
      d.style.animationDuration = 10 + Math.random() * 20 + 's';
      d.style.opacity = Math.random() * 0.2;
      document.body.appendChild(d);
    }

    // ===== Логика тарифов =====
    const daysBtns = document.querySelectorAll('.day-btn');
    const priceDisplay = document.getElementById('priceDisplay');
    const buyBtn = document.getElementById('buyBtn');
    const modalOverlay = document.getElementById('modalOverlay');
    const modalDays = document.getElementById('modalDays');
    const modalPrice = document.getElementById('modalPrice');

    let selectedDays = 30;
    let selectedPrice = 549;

    daysBtns.forEach(btn => {
      btn.addEventListener('click', () => {
        daysBtns.forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
        selectedDays = parseInt(btn.dataset.days);
        selectedPrice = parseInt(btn.dataset.price);
        priceDisplay.innerHTML = `${selectedPrice} <small>₽</small>`;
      });
    });

    buyBtn.addEventListener('click', () => {
      modalDays.textContent = selectedDays + ' дней';
      modalPrice.textContent = selectedPrice + ' ₽';
      modalOverlay.classList.add('show');
    });

    window.closeModal = function() {
      modalOverlay.classList.remove('show');
    };

    modalOverlay.addEventListener('click', (e) => {
      if (e.target === modalOverlay) closeModal();
    });
  </script>
</body>
</html>
