<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Podium.gg — Premium DLC</title>

  <!-- Шрифты и иконки -->
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;900&display=swap" rel="stylesheet" />
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css" />

  <!-- EmailJS -->
  <script src="https://cdn.jsdelivr.net/npm/@emailjs/browser@3/dist/email.min.js"></script>

  <style>
    /* ===== Глобальные сбросы и базовые стили ===== */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: 'Inter', sans-serif;
    }

    body {
      background: #0a0a0f;
      overflow-x: hidden;
      color: #fff;
      line-height: 1.6;
    }

    canvas {
      position: fixed;
      inset: 0;
      z-index: -2;
      pointer-events: none;
    }

    #noise {
      position: fixed;
      inset: 0;
      pointer-events: none;
      opacity: 0.04;
      background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='160' height='160'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.8'/%3E%3C/filter%3E%3Crect width='160' height='160' filter='url(%23n)'/%3E%3C/svg%3E");
      mix-blend-mode: overlay;
      z-index: -1;
    }

    .cursorGlow {
      position: fixed;
      width: 600px;
      height: 600px;
      border-radius: 50%;
      pointer-events: none;
      background: radial-gradient(circle, rgba(120, 80, 255, 0.15), transparent 70%);
      transform: translate(-50%, -50%);
      z-index: -1;
      transition: opacity 0.3s ease;
    }

    /* ===== Анимации появления ===== */
    @keyframes fadeInUp {
      from {
        opacity: 0;
        transform: translateY(30px) scale(0.98);
      }
      to {
        opacity: 1;
        transform: translateY(0) scale(1);
      }
    }

    .animate-on-load {
      animation: fadeInUp 0.8s ease forwards;
    }

    /* ===== Навигация ===== */
    nav {
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      padding: 18px 10%;
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: rgba(10, 10, 15, 0.7);
      backdrop-filter: blur(20px);
      border-bottom: 1px solid rgba(255, 255, 255, 0.05);
      z-index: 100;
      transition: background 0.4s ease;
    }

    nav:hover {
      background: rgba(10, 10, 15, 0.9);
    }

    .logo {
      font-size: 26px;
      font-weight: 900;
      letter-spacing: 2px;
      background: linear-gradient(135deg, #fff, #a78bfa);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
      text-shadow: 0 0 30px rgba(167, 139, 250, 0.3);
    }

    .nav-links {
      display: flex;
      gap: 35px;
      align-items: center;
      flex-wrap: wrap;
    }

    .nav-links a {
      color: rgba(255, 255, 255, 0.6);
      text-decoration: none;
      font-weight: 500;
      font-size: 15px;
      transition: all 0.3s ease;
      position: relative;
      cursor: pointer;
      letter-spacing: 0.3px;
    }

    .nav-links a::after {
      content: '';
      position: absolute;
      bottom: -4px;
      left: 0;
      width: 0;
      height: 2px;
      background: linear-gradient(90deg, #a78bfa, #7c3aed);
      transition: width 0.3s ease;
    }

    .nav-links a:hover {
      color: #fff;
    }

    .nav-links a:hover::after {
      width: 100%;
    }

    .nav-links .auth-link {
      background: linear-gradient(135deg, #a78bfa, #7c3aed);
      padding: 8px 24px;
      border-radius: 50px;
      color: #fff !important;
      font-weight: 600;
      transition: all 0.3s ease;
      box-shadow: 0 0 20px rgba(167, 139, 250, 0.3);
    }

    .nav-links .auth-link::after {
      display: none;
    }

    .nav-links .auth-link:hover {
      transform: scale(1.05);
      box-shadow: 0 0 40px rgba(167, 139, 250, 0.5);
    }

    /* ===== Хедер ===== */
    header {
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      text-align: center;
      padding-top: 80px;
      position: relative;
    }

    h1 {
      font-size: clamp(3.5rem, 12vw, 7rem);
      font-weight: 900;
      letter-spacing: 6px;
      animation: glitch 6s infinite;
      background: linear-gradient(135deg, #fff 30%, #a78bfa 70%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
      text-shadow: 0 0 60px rgba(167, 139, 250, 0.2);
    }

    .subtitle {
      margin-top: 20px;
      opacity: 0.8;
      font-size: clamp(1rem, 2.5vw, 1.5rem);
      letter-spacing: 4px;
      text-transform: uppercase;
      color: rgba(255, 255, 255, 0.7);
    }

    .btn-primary {
      margin-top: 50px;
      padding: 18px 60px;
      font-size: 1.1rem;
      border: none;
      cursor: pointer;
      background: linear-gradient(135deg, #fff, #e0e0e0);
      color: #0a0a0f;
      font-weight: 700;
      border-radius: 60px;
      transition: all 0.4s ease;
      box-shadow: 0 0 30px rgba(255, 255, 255, 0.1);
      letter-spacing: 1px;
    }

    .btn-primary:hover {
      transform: scale(1.08) translateY(-3px);
      box-shadow: 0 0 80px rgba(167, 139, 250, 0.4);
    }

    /* ===== Секции ===== */
    section {
      padding: 120px 10% 80px;
      scroll-margin-top: 100px;
    }

    .section-title {
      font-size: clamp(2.5rem, 6vw, 3.8rem);
      font-weight: 900;
      margin-bottom: 50px;
      text-align: center;
      background: linear-gradient(135deg, #fff, #a78bfa);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
      letter-spacing: 2px;
    }

    /* ===== Карточки продуктов ===== */
    .products {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
      gap: 40px;
      max-width: 1200px;
      margin: 0 auto;
    }

    .product-card {
      background: rgba(255, 255, 255, 0.03);
      backdrop-filter: blur(16px);
      border: 1px solid rgba(255, 255, 255, 0.08);
      border-radius: 40px;
      padding: 40px 25px;
      text-align: center;
      transition: all 0.5s cubic-bezier(0.23, 1, 0.32, 1);
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 22px;
      position: relative;
      overflow: hidden;
    }

    .product-card::before {
      content: '';
      position: absolute;
      inset: -2px;
      border-radius: 42px;
      padding: 2px;
      background: linear-gradient(135deg, rgba(167, 139, 250, 0.3), transparent, rgba(167, 139, 250, 0.3));
      -webkit-mask: linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0);
      -webkit-mask-composite: xor;
      mask-composite: exclude;
      pointer-events: none;
      opacity: 0;
      transition: opacity 0.6s ease;
    }

    .product-card:hover::before {
      opacity: 1;
    }

    .product-card:hover {
      transform: translateY(-16px) scale(1.02);
      box-shadow: 0 30px 80px rgba(167, 139, 250, 0.15);
      border-color: rgba(167, 139, 250, 0.3);
    }

    .product-card h3 {
      font-size: 1.5rem;
      font-weight: 700;
      letter-spacing: 0.5px;
      background: linear-gradient(135deg, #fff, #a78bfa);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }

    .product-card .days-selector {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 8px;
    }

    .product-card .day-btn {
      padding: 6px 14px;
      border-radius: 30px;
      border: 1px solid rgba(255, 255, 255, 0.15);
      background: rgba(255, 255, 255, 0.04);
      color: rgba(255, 255, 255, 0.6);
      font-weight: 600;
      font-size: 0.8rem;
      cursor: pointer;
      transition: all 0.3s ease;
    }

    .product-card .day-btn:hover {
      border-color: rgba(167, 139, 250, 0.6);
      color: #fff;
      background: rgba(167, 139, 250, 0.1);
    }

    .product-card .day-btn.active {
      background: linear-gradient(135deg, #a78bfa, #7c3aed);
      border-color: transparent;
      color: #fff;
      box-shadow: 0 0 30px rgba(167, 139, 250, 0.3);
    }

    .product-card .price-display {
      font-size: 2.8rem;
      font-weight: 900;
      letter-spacing: 1px;
      background: linear-gradient(135deg, #fff, #e0e0e0);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }

    .product-card .price-display small {
      font-size: 1.2rem;
      opacity: 0.6;
    }

    /* Способ оплаты */
    .payment-method {
      display: inline-flex;
      align-items: center;
      gap: 12px;
      padding: 12px 32px;
      border-radius: 60px;
      border: 1px solid rgba(255, 255, 255, 0.15);
      background: rgba(255, 255, 255, 0.04);
      color: rgba(255, 255, 255, 0.7);
      font-weight: 600;
      font-size: 1rem;
      cursor: pointer;
      transition: all 0.4s ease;
      user-select: none;
    }

    .payment-method:hover {
      border-color: rgba(167, 139, 250, 0.5);
      background: rgba(167, 139, 250, 0.08);
      transform: scale(1.03);
    }

    .payment-method.active {
      border-color: #a78bfa;
      background: rgba(167, 139, 250, 0.15);
      box-shadow: 0 0 40px rgba(167, 139, 250, 0.2);
      color: #fff;
    }

    .payment-method i {
      font-size: 1.6rem;
      color: #fbbf24;
    }

    .product-card .btn-buy {
      padding: 14px 44px;
      border-radius: 60px;
      border: none;
      font-weight: 700;
      font-size: 1rem;
      cursor: pointer;
      transition: all 0.4s ease;
      background: rgba(255, 255, 255, 0.06);
      color: rgba(255, 255, 255, 0.3);
      pointer-events: none;
      opacity: 0.5;
      width: 100%;
      max-width: 220px;
      letter-spacing: 0.5px;
    }

    .product-card .btn-buy.active {
      background: linear-gradient(135deg, #a78bfa, #7c3aed);
      color: #fff;
      pointer-events: auto;
      opacity: 1;
      box-shadow: 0 0 30px rgba(167, 139, 250, 0.3);
    }

    .product-card .btn-buy.active:hover {
      transform: scale(1.05);
      box-shadow: 0 0 60px rgba(167, 139, 250, 0.5);
    }

    /* ===== Поддержка ===== */
    .support-wrapper {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 30px;
    }

    .support-wrapper p {
      font-size: 1.2rem;
      opacity: 0.7;
      max-width: 500px;
      text-align: center;
    }

    .discord-link {
      display: inline-flex;
      align-items: center;
      gap: 16px;
      padding: 18px 48px;
      border-radius: 60px;
      background: linear-gradient(135deg, #5865f2, #4752c4);
      color: #fff;
      font-size: 1.4rem;
      font-weight: 700;
      text-decoration: none;
      transition: all 0.4s ease;
      box-shadow: 0 0 40px rgba(88, 101, 242, 0.3);
    }

    .discord-link i {
      font-size: 2.2rem;
    }

    .discord-link:hover {
      transform: scale(1.08) translateY(-3px);
      box-shadow: 0 0 80px rgba(88, 101, 242, 0.5);
    }

    /* ===== Футер ===== */
    footer {
      padding: 80px 10%;
      text-align: center;
      opacity: 0.5;
      border-top: 1px solid rgba(255, 255, 255, 0.04);
      font-weight: 300;
      letter-spacing: 1px;
      font-size: 0.9rem;
    }

    /* ===== Модальные окна ===== */
    .modal-overlay {
      display: none;
      position: fixed;
      inset: 0;
      background: rgba(0, 0, 0, 0.7);
      backdrop-filter: blur(16px);
      justify-content: center;
      align-items: center;
      z-index: 999;
      animation: fadeIn 0.3s ease;
    }

    .modal-overlay.show {
      display: flex;
    }

    @keyframes fadeIn {
      from {
        opacity: 0;
        transform: scale(0.95);
      }
      to {
        opacity: 1;
        transform: scale(1);
      }
    }

    .modal {
      background: #16161f;
      border: 1px solid rgba(255, 255, 255, 0.08);
      border-radius: 50px;
      padding: 45px 40px;
      max-width: 460px;
      width: 90%;
      text-align: center;
      max-height: 90vh;
      overflow-y: auto;
      box-shadow: 0 40px 120px rgba(0, 0, 0, 0.6);
    }

    .modal .tabs {
      display: flex;
      gap: 24px;
      justify-content: center;
      margin-bottom: 35px;
    }

    .modal .tabs span {
      font-size: 1.1rem;
      font-weight: 600;
      opacity: 0.4;
      cursor: pointer;
      transition: all 0.3s ease;
      padding-bottom: 4px;
    }

    .modal .tabs span.active {
      opacity: 1;
      border-bottom: 2px solid #a78bfa;
      padding-bottom: 4px;
      color: #fff;
    }

    .modal form {
      display: flex;
      flex-direction: column;
      gap: 18px;
    }

    .modal form input {
      padding: 16px 24px;
      border-radius: 40px;
      border: 1px solid rgba(255, 255, 255, 0.08);
      background: rgba(255, 255, 255, 0.04);
      color: #fff;
      font-size: 1rem;
      outline: none;
      transition: all 0.3s ease;
    }

    .modal form input:focus {
      border-color: #a78bfa;
      background: rgba(167, 139, 250, 0.05);
      box-shadow: 0 0 30px rgba(167, 139, 250, 0.1);
    }

    .modal form button {
      padding: 16px;
      border-radius: 40px;
      border: none;
      background: linear-gradient(135deg, #a78bfa, #7c3aed);
      color: #fff;
      font-weight: 700;
      font-size: 1.1rem;
      cursor: pointer;
      transition: all 0.3s ease;
      box-shadow: 0 0 30px rgba(167, 139, 250, 0.2);
    }

    .modal form button:hover {
      transform: scale(1.02);
      box-shadow: 0 0 50px rgba(167, 139, 250, 0.4);
    }

    .modal .error {
      color: #f87171;
      font-size: 0.85rem;
      margin-top: -8px;
    }

    .modal .success {
      color: #34d399;
      font-size: 0.85rem;
    }

    .modal h3 {
      font-size: 1.8rem;
      margin-bottom: 15px;
      background: linear-gradient(135deg, #fff, #a78bfa);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }

    /* ===== Кабинет ===== */
    .cabinet {
      display: none;
      padding: 140px 10% 80px;
      max-width: 720px;
      margin: 0 auto;
    }

    .cabinet.show {
      display: block;
      animation: fadeInUp 0.6s ease;
    }

    .cabinet .info-block {
      background: rgba(255, 255, 255, 0.03);
      backdrop-filter: blur(16px);
      border: 1px solid rgba(255, 255, 255, 0.06);
      border-radius: 40px;
      padding: 45px 40px;
      margin-bottom: 35px;
    }

    .cabinet .info-block .row {
      display: flex;
      justify-content: space-between;
      padding: 16px 0;
      border-bottom: 1px solid rgba(255, 255, 255, 0.04);
    }

    .cabinet .info-block .row:last-child {
      border-bottom: none;
    }

    .cabinet .info-block .label {
      opacity: 0.5;
      font-weight: 400;
    }

    .cabinet .info-block .value {
      font-weight: 600;
      color: #fff;
    }

    .cabinet .info-block .value .masked-email {
      cursor: help;
      border-bottom: 1px dashed rgba(255, 255, 255, 0.2);
    }

    .cabinet .actions {
      display: flex;
      flex-wrap: wrap;
      gap: 16px;
      justify-content: center;
      margin-top: 20px;
    }

    .cabinet .actions button {
      padding: 14px 32px;
      border-radius: 60px;
      border: none;
      font-weight: 600;
      font-size: 0.95rem;
      cursor: pointer;
      transition: all 0.3s ease;
      background: rgba(255, 255, 255, 0.06);
      color: #fff;
      border: 1px solid rgba(255, 255, 255, 0.08);
    }

    .cabinet .actions button:hover {
      background: rgba(255, 255, 255, 0.12);
      transform: scale(1.04);
    }

    .cabinet .actions .btn-buy-client {
      background: linear-gradient(135deg, #a78bfa, #7c3aed);
      border: none;
      color: #fff;
      box-shadow: 0 0 30px rgba(167, 139, 250, 0.2);
    }

    .cabinet .actions .btn-buy-client:hover {
      box-shadow: 0 0 50px rgba(167, 139, 250, 0.4);
    }

    .cabinet .actions .btn-logout {
      background: rgba(239, 68, 68, 0.15);
      border-color: rgba(239, 68, 68, 0.2);
      color: #f87171;
    }

    .cabinet .actions .btn-logout:hover {
      background: rgba(239, 68, 68, 0.25);
    }

    /* ===== Анимация глитча ===== */
    @keyframes glitch {
      0%,
      100% {
        text-shadow: 0 0 10px rgba(167, 139, 250, 0.5), 0 0 40px rgba(167, 139, 250, 0.2);
      }
      20% {
        transform: translateX(3px) skewX(-1deg);
      }
      21% {
        transform: translateX(-3px) skewX(1deg);
      }
      40% {
        filter: blur(1px);
      }
      60% {
        transform: rotate(0.5deg);
      }
      80% {
        letter-spacing: 12px;
      }
    }

    /* ===== Плавающие круги ===== */
    .floating {
      position: absolute;
      border: 1px solid rgba(167, 139, 250, 0.08);
      border-radius: 50%;
      animation: float linear infinite;
      pointer-events: none;
    }

    @keyframes float {
      from {
        transform: translateY(120vh) rotate(0deg);
      }
      to {
        transform: translateY(-150vh) rotate(720deg);
      }
    }

    /* ===== Адаптивность ===== */
    @media (max-width: 768px) {
      nav {
        padding: 16px 6%;
        flex-direction: column;
        gap: 12px;
      }
      .nav-links {
        gap: 20px;
        justify-content: center;
      }
      .nav-links a {
        font-size: 14px;
      }
      .nav-links .auth-link {
        padding: 6px 20px;
        font-size: 13px;
      }
      .section-title {
        font-size: 2.2rem;
      }
      .products {
        grid-template-columns: 1fr;
        gap: 30px;
      }
      .product-card {
        padding: 30px 20px;
      }
      .modal {
        padding: 30px 25px;
      }
      .cabinet {
        padding: 120px 6% 60px;
      }
      .cabinet .info-block {
        padding: 25px 20px;
      }
    }

    @media (max-width: 480px) {
      .nav-links a {
        font-size: 12px;
      }
      .product-card .day-btn {
        font-size: 11px;
        padding: 4px 10px;
      }
      .product-card .price-display {
        font-size: 2.2rem;
      }
      .btn-primary {
        padding: 14px 40px;
        font-size: 1rem;
      }
      .discord-link {
        font-size: 1.1rem;
        padding: 14px 30px;
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
      <a id="navPlans">Тарифы</a>
      <a id="navSupport">Поддержка</a>
      <span class="auth-link" id="authLink">Войти</span>
    </div>
  </nav>

  <!-- Основной контент -->
  <div id="mainContent">
    <header>
      <h1 class="animate-on-load">PODIUM.GG</h1>
      <div class="subtitle animate-on-load" style="animation-delay:0.2s;">PREMIUM DLC EXPERIENCE</div>
      <button class="btn-primary animate-on-load" style="animation-delay:0.4s;" onclick="document.getElementById('plans').scrollIntoView({behavior:'smooth'})">
        ПОСМОТРЕТЬ ТАРИФЫ
      </button>
    </header>

    <section id="plans">
      <h2 class="section-title">Выберите продукт</h2>
      <div class="products">
        <!-- макросы -->
        <div class="product-card animate-on-load" data-product="macros" style="animation-delay:0.1s;">
          <h3>Rustex Remake macros DLC</h3>
          <div class="days-selector">
            <button class="day-btn active" data-days="30" data-price="549">30 дн.</button>
            <button class="day-btn" data-days="14" data-price="349">14 дн.</button>
            <button class="day-btn" data-days="7" data-price="229">7 дн.</button>
            <button class="day-btn" data-days="3" data-price="159">3 дн.</button>
            <button class="day-btn" data-days="1" data-price="99">1 дн.</button>
          </div>
          <div class="price-display">549 <small>₽</small></div>
          <div class="payment-method"><i class="fas fa-coins"></i> FunPay</div>
          <button class="btn-buy product-buy" disabled>Купить</button>
        </div>

        <!-- премиум -->
        <div class="product-card animate-on-load" data-product="premium" style="animation-delay:0.2s;">
          <h3>Rustex Remake premium DLC</h3>
          <div class="days-selector">
            <button class="day-btn active" data-days="30" data-price="1090">30 дн.</button>
            <button class="day-btn" data-days="14" data-price="849">14 дн.</button>
            <button class="day-btn" data-days="7" data-price="649">7 дн.</button>
            <button class="day-btn" data-days="3" data-price="349">3 дн.</button>
            <button class="day-btn" data-days="1" data-price="139">1 дн.</button>
          </div>
          <div class="price-display">1090 <small>₽</small></div>
          <div class="payment-method"><i class="fas fa-coins"></i> FunPay</div>
          <button class="btn-buy product-buy" disabled>Купить</button>
        </div>

        <!-- сброс HWID -->
        <div class="product-card animate-on-load" data-product="hwid" style="animation-delay:0.3s;">
          <h3>Reset HWID</h3>
          <div class="price-display">119 <small>₽</small></div>
          <div class="payment-method"><i class="fas fa-coins"></i> FunPay</div>
          <button class="btn-buy product-buy" disabled>Купить</button>
        </div>
      </div>
    </section>

    <section id="support">
      <h2 class="section-title">Поддержка</h2>
      <div class="support-wrapper">
        <p>Присоединяйтесь к нашему Discord-серверу — там мы всегда на связи</p>
        <a href="https://discord.gg/sUNGPKadB" target="_blank" class="discord-link">
          <i class="fab fa-discord"></i> Discord
        </a>
      </div>
    </section>

    <footer>
      © 2026 Podium.gg Все права защищены.
    </footer>
  </div>

  <!-- Кабинет -->
  <div class="cabinet" id="cabinet">
    <div class="info-block">
      <div class="row"><span class="label">UID</span><span class="value" id="cabinetUid">—</span></div>
      <div class="row"><span class="label">Логин</span><span class="value" id="cabinetLogin">—</span></div>
      <div class="row"><span class="label">Почта</span><span class="value"><span class="masked-email" id="cabinetEmail" title="полный email">***</span></span></div>
      <div class="row"><span class="label">HWID</span><span class="value" id="cabinetHwid">Не привязан</span></div>
      <div class="row"><span class="label">Подписка</span><span class="value" id="cabinetSubscription">Не оформлена</span></div>
    </div>
    <div class="actions">
      <button id="downloadLauncher">Скачать лаунчер</button>
      <button class="btn-buy-client" id="buyClientBtn">Купить клиент</button>
      <button class="btn-logout" id="logoutBtn">Выйти</button>
    </div>
  </div>

  <!-- Модальное окно входа/регистрации -->
  <div class="modal-overlay" id="authModal">
    <div class="modal">
      <div class="tabs">
        <span class="active" data-tab="login">Вход</span>
        <span data-tab="register">Регистрация</span>
      </div>
      <form id="loginForm">
        <input type="text" placeholder="Логин" id="loginLogin" required />
        <input type="password" placeholder="Пароль" id="loginPassword" required />
        <div class="error" id="loginError"></div>
        <button type="submit">Войти</button>
      </form>
      <form id="registerForm" style="display:none;">
        <input type="email" placeholder="Email" id="regEmail" required />
        <input type="text" placeholder="Логин" id="regLogin" required />
        <input type="password" placeholder="Пароль" id="regPassword" required />
        <input type="password" placeholder="Подтвердите пароль" id="regPasswordConfirm" required />
        <div class="error" id="regError"></div>
        <button type="submit">Зарегистрироваться</button>
      </form>
    </div>
  </div>

  <!-- Модальное окно подтверждения email -->
  <div class="modal-overlay" id="verifyModal">
    <div class="modal">
      <h3>Подтверждение email</h3>
      <p>На ваш email отправлен код подтверждения. Введите его ниже.</p>
      <p style="color: #fbbf24; font-size: 0.9rem; margin: 8px 0;">(Демо-код также показан на экране: <span id="verifyCodeDisplay" style="font-weight:bold;font-size:1.2rem;"></span>)</p>
      <form id="verifyForm">
        <input type="text" placeholder="Код подтверждения" id="verifyCode" maxlength="6" required />
        <div class="error" id="verifyError"></div>
        <button type="submit">Подтвердить</button>
      </form>
      <p style="margin-top:12px;font-size:0.8rem;opacity:0.4;">Если письмо не пришло, проверьте спам или повторите регистрацию.</p>
    </div>
  </div>

  <script>
    // ============================================================
    // ⚙️ НАСТРОЙКА EMAILJS (ЗАМЕНИТЕ SERVICE_ID И TEMPLATE_ID)
    // ============================================================
    const EMAILJS_CONFIG = {
      publicKey: 'OOo-5hbLUAC1aAZeb',
      serviceID: 'service_abc123',   // <--- ЗАМЕНИТЕ НА ВАШ
      templateID: 'template_xyz789'  // <--- ЗАМЕНИТЕ НА ВАШ
    };

    // Инициализация EmailJS (если ключи не пустые)
    if (EMAILJS_CONFIG.publicKey && EMAILJS_CONFIG.publicKey !== 'YOUR_PUBLIC_KEY') {
      emailjs.init(EMAILJS_CONFIG.publicKey);
    }

    // ============================================================
    // 1. Фоновые эффекты
    // ============================================================
    const glow = document.querySelector('.cursorGlow');
    window.addEventListener('mousemove', e => {
      glow.style.left = e.clientX + 'px';
      glow.style.top = e.clientY + 'px';
    });

    const c = document.getElementById('bg');
    const ctx = c.getContext('2d');
    function resize() {
      c.width = window.innerWidth;
      c.height = window.innerHeight;
    }
    resize();
    window.addEventListener('resize', resize);

    const stars = [];
    for (let i = 0; i < 300; i++) {
      stars.push({
        x: Math.random() * c.width,
        y: Math.random() * c.height,
        r: Math.random() * 2.5 + 0.5,
        v: Math.random() * 0.4 + 0.1,
        alpha: Math.random() * 0.8 + 0.2
      });
    }

    function draw() {
      ctx.clearRect(0, 0, c.width, c.height);
      for (const s of stars) {
        ctx.beginPath();
        ctx.arc(s.x, s.y, s.r, 0, Math.PI * 2);
        ctx.fillStyle = `rgba(255,255,255,${s.alpha})`;
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

    for (let i = 0; i < 45; i++) {
      const d = document.createElement('div');
      d.className = 'floating';
      const s = Math.random() * 280 + 40;
      d.style.width = s + 'px';
      d.style.height = s + 'px';
      d.style.left = Math.random() * 100 + 'vw';
      d.style.animationDuration = 15 + Math.random() * 30 + 's';
      d.style.opacity = Math.random() * 0.12 + 0.03;
      document.body.appendChild(d);
    }

    // ============================================================
    // 2. Логика продуктов (тарифы + оплата)
    // ============================================================
    const FUNPAY_URL = 'https://funpay.com/users/20739931/';

    document.querySelectorAll('.product-card').forEach(card => {
      const productType = card.dataset.product;
      const daysBtns = card.querySelectorAll('.day-btn');
      const priceDisplay = card.querySelector('.price-display');
      const paymentMethod = card.querySelector('.payment-method');
      const buyBtn = card.querySelector('.product-buy');

      function updateBuyButton() {
        const dayActive = card.querySelector('.day-btn.active');
        const paymentActive = paymentMethod.classList.contains('active');
        const daySelected = (productType === 'hwid') ? true : (dayActive !== null);
        if (daySelected && paymentActive) {
          buyBtn.disabled = false;
          buyBtn.classList.add('active');
        } else {
          buyBtn.disabled = true;
          buyBtn.classList.remove('active');
        }
      }

      daysBtns.forEach(btn => {
        btn.addEventListener('click', () => {
          daysBtns.forEach(b => b.classList.remove('active'));
          btn.classList.add('active');
          const price = btn.dataset.price;
          priceDisplay.innerHTML = `${price} <small>₽</small>`;
          updateBuyButton();
        });
      });

      paymentMethod.addEventListener('click', () => {
        paymentMethod.classList.toggle('active');
        updateBuyButton();
      });

      buyBtn.addEventListener('click', () => {
        if (!buyBtn.disabled) {
          window.open(FUNPAY_URL, '_blank');
        }
      });

      // инициализация
      const activeDay = card.querySelector('.day-btn.active');
      if (activeDay) {
        priceDisplay.innerHTML = `${activeDay.dataset.price} <small>₽</small>`;
      }
      buyBtn.disabled = true;
      buyBtn.classList.remove('active');
    });

    // ============================================================
    // 3. Регистрация / вход / кабинет + EmailJS
    // ============================================================
    let users = JSON.parse(localStorage.getItem('podium_users')) || [];
    let currentUser = JSON.parse(localStorage.getItem('podium_currentUser')) || null;
    let pendingVerification = null;

    const authLink = document.getElementById('authLink');
    const authModal = document.getElementById('authModal');
    const loginForm = document.getElementById('loginForm');
    const registerForm = document.getElementById('registerForm');
    const verifyModal = document.getElementById('verifyModal');
    const verifyForm = document.getElementById('verifyForm');
    const verifyCodeInput = document.getElementById('verifyCode');
    const verifyCodeDisplay = document.getElementById('verifyCodeDisplay');
    const verifyError = document.getElementById('verifyError');
    const tabs = document.querySelectorAll('.tabs span');
    const loginError = document.getElementById('loginError');
    const regError = document.getElementById('regError');

    const cabinet = document.getElementById('cabinet');
    const mainContent = document.getElementById('mainContent');
    const cabinetUid = document.getElementById('cabinetUid');
    const cabinetLogin = document.getElementById('cabinetLogin');
    const cabinetEmail = document.getElementById('cabinetEmail');
    const cabinetHwid = document.getElementById('cabinetHwid');
    const cabinetSubscription = document.getElementById('cabinetSubscription');
    const logoutBtn = document.getElementById('logoutBtn');
    const downloadLauncher = document.getElementById('downloadLauncher');
    const buyClientBtn = document.getElementById('buyClientBtn');

    const navPlans = document.getElementById('navPlans');
    const navSupport = document.getElementById('navSupport');

    function maskEmail(email) {
      if (!email) return '***';
      const at = email.indexOf('@');
      if (at <= 1) return '***' + email.substring(at);
      return email[0] + '***' + email.substring(at);
    }

    function fillCabinetData() {
      if (!currentUser) return;
      cabinetUid.textContent = currentUser.uid || '—';
      cabinetLogin.textContent = currentUser.login || '—';
      const fullEmail = currentUser.email || '—';
      cabinetEmail.textContent = maskEmail(fullEmail);
      cabinetEmail.title = fullEmail;
      cabinetHwid.textContent = currentUser.hwid || 'Не привязан';
      if (currentUser.subscription && currentUser.subscription.expires) {
        const exp = new Date(currentUser.subscription.expires);
        if (exp > new Date()) {
          cabinetSubscription.textContent = 'Активна до: ' + exp.toLocaleDateString('ru-RU');
        } else {
          cabinetSubscription.textContent = 'Истекла';
        }
      } else {
        cabinetSubscription.textContent = 'Не оформлена';
      }
    }

    function showCabinet() {
      if (!currentUser) return;
      mainContent.style.display = 'none';
      cabinet.classList.add('show');
      fillCabinetData();
    }

    function hideCabinet() {
      mainContent.style.display = 'block';
      cabinet.classList.remove('show');
    }

    function updateUI() {
      if (currentUser) {
        authLink.textContent = 'Кабинет';
      } else {
        authLink.textContent = 'Войти';
        hideCabinet();
      }
    }

    function saveUsers() { localStorage.setItem('podium_users', JSON.stringify(users)); }
    function saveCurrentUser() { localStorage.setItem('podium_currentUser', JSON.stringify(currentUser)); }
    function generateUid() { return 'UID-' + Math.random().toString(36).substring(2, 10).toUpperCase(); }
    function generateVerifyCode() { return Math.floor(100000 + Math.random() * 900000).toString(); }

    // Переключение вкладок
    tabs.forEach(tab => {
      tab.addEventListener('click', () => {
        tabs.forEach(t => t.classList.remove('active'));
        tab.classList.add('active');
        const target = tab.dataset.tab;
        if (target === 'login') {
          loginForm.style.display = 'flex';
          registerForm.style.display = 'none';
        } else {
          loginForm.style.display = 'none';
          registerForm.style.display = 'flex';
        }
        loginError.textContent = '';
        regError.textContent = '';
      });
    });

    // Открытие модалки входа
    authLink.addEventListener('click', (e) => {
      e.preventDefault();
      if (currentUser) {
        showCabinet();
        return;
      }
      authModal.classList.add('show');
      loginForm.style.display = 'flex';
      registerForm.style.display = 'none';
      tabs.forEach(t => t.classList.remove('active'));
      document.querySelector('.tabs span[data-tab="login"]').classList.add('active');
      loginError.textContent = '';
      regError.textContent = '';
    });

    // Закрытие модалок по клику вне
    authModal.addEventListener('click', (e) => {
      if (e.target === authModal) authModal.classList.remove('show');
    });
    verifyModal.addEventListener('click', (e) => {
      if (e.target === verifyModal) {
        verifyModal.classList.remove('show');
        pendingVerification = null;
      }
    });

    // === Регистрация ===
    registerForm.addEventListener('submit', async (e) => {
      e.preventDefault();
      const email = document.getElementById('regEmail').value.trim();
      const login = document.getElementById('regLogin').value.trim();
      const password = document.getElementById('regPassword').value;
      const confirm = document.getElementById('regPasswordConfirm').value;
      regError.textContent = '';

      if (!email || !login || !password || !confirm) {
        regError.textContent = 'Заполните все поля.';
        return;
      }
      if (password.length < 6) {
        regError.textContent = 'Пароль должен содержать минимум 6 символов.';
        return;
      }
      if (password !== confirm) {
        regError.textContent = 'Пароли не совпадают.';
        return;
      }
      if (users.find(u => u.login === login)) {
        regError.textContent = 'Пользователь с таким логином уже существует.';
        return;
      }
      if (users.find(u => u.email === email)) {
        regError.textContent = 'Пользователь с таким email уже существует.';
        return;
      }

      const newUser = {
        uid: generateUid(),
        email: email,
        login: login,
        password: password,
        hwid: 'Не привязан',
        subscription: { expires: null },
        verified: false
      };
      const code = generateVerifyCode();
      verifyCodeDisplay.textContent = code;
      pendingVerification = { user: newUser, code: code };

      // Отправка письма через EmailJS
      if (EMAILJS_CONFIG.publicKey && EMAILJS_CONFIG.publicKey !== 'YOUR_PUBLIC_KEY') {
        try {
          const templateParams = {
            to_email: email,
            to_name: login,
            verification_code: code,
            site_name: 'Podium.gg'
          };
          await emailjs.send(EMAILJS_CONFIG.serviceID, EMAILJS_CONFIG.templateID, templateParams);
          console.log('✅ Email отправлен');
        } catch (err) {
          console.warn('⚠️ Ошибка отправки email:', err);
          regError.textContent = 'Не удалось отправить письмо, но код показан на экране.';
        }
      } else {
        console.warn('⚠️ EmailJS не настроен, код только на экране.');
      }

      authModal.classList.remove('show');
      verifyModal.classList.add('show');
      verifyCodeInput.value = '';
      verifyError.textContent = '';
    });

    // === Подтверждение email ===
    verifyForm.addEventListener('submit', (e) => {
      e.preventDefault();
      const inputCode = verifyCodeInput.value.trim();
      if (!pendingVerification) {
        verifyError.textContent = 'Ошибка: нет ожидающей верификации.';
        return;
      }
      if (inputCode === pendingVerification.code) {
        const user = pendingVerification.user;
        user.verified = true;
        users.push(user);
        saveUsers();
        currentUser = user;
        saveCurrentUser();
        verifyModal.classList.remove('show');
        pendingVerification = null;
        updateUI();
        hideCabinet();
        alert('✅ Регистрация успешна! Добро пожаловать.');
      } else {
        verifyError.textContent = 'Неверный код подтверждения.';
      }
    });

    // === Вход ===
    loginForm.addEventListener('submit', (e) => {
      e.preventDefault();
      const login = document.getElementById('loginLogin').value.trim();
      const password = document.getElementById('loginPassword').value;
      loginError.textContent = '';

      if (!login || !password) {
        loginError.textContent = 'Введите логин и пароль.';
        return;
      }
      const user = users.find(u => u.login === login && u.password === password);
      if (!user) {
        loginError.textContent = 'Неверный логин или пароль.';
        return;
      }
      if (!user.verified) {
        loginError.textContent = 'Email не подтверждён. Проверьте почту.';
        return;
      }
      currentUser = user;
      saveCurrentUser();
      authModal.classList.remove('show');
      updateUI();
      hideCabinet();
    });

    // Выход
    logoutBtn.addEventListener('click', () => {
      currentUser = null;
      localStorage.removeItem('podium_currentUser');
      updateUI();
      hideCabinet();
      window.scrollTo({ top: 0, behavior: 'smooth' });
    });

    // Кнопка "Скачать лаунчер"
    downloadLauncher.addEventListener('click', () => {
      alert('Скачивание лаунчера начнется... (демо)');
    });

    // "Купить клиент" – скрыть кабинет и скролл к тарифам
    buyClientBtn.addEventListener('click', () => {
      hideCabinet();
      document.getElementById('plans').scrollIntoView({ behavior: 'smooth' });
    });

    // Навигация
    navPlans.addEventListener('click', (e) => {
      e.preventDefault();
      if (currentUser) hideCabinet();
      document.getElementById('plans').scrollIntoView({ behavior: 'smooth' });
    });
    navSupport.addEventListener('click', (e) => {
      e.preventDefault();
      if (currentUser) hideCabinet();
      document.getElementById('support').scrollIntoView({ behavior: 'smooth' });
    });

    // Инициализация
    if (currentUser) {
      updateUI();
      hideCabinet();
    } else {
      updateUI();
      hideCabinet();
    }

    console.log('✅ Сайт обновлён с улучшенным дизайном и анимациями.');
  </script>
</body>
</html>
