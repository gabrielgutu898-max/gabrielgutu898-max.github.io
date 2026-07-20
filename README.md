
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Podium.gg — Premium DLC</title>

  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;500;700;900&display=swap" rel="stylesheet" />
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css" />

  <style>
    /* ---------- Глобальные стили ---------- */
    * { margin:0; padding:0; box-sizing:border-box; font-family:'Inter',sans-serif; }
    body { background:#000; overflow-x:hidden; color:#fff; }
    canvas { position:fixed; inset:0; z-index:-2; }
    #noise { position:fixed; inset:0; pointer-events:none; opacity:.06; background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='160' height='160'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.8'/%3E%3C/filter%3E%3Crect width='160' height='160' filter='url(%23n)'/%3E%3C/svg%3E"); mix-blend-mode:overlay; z-index:-1; }
    .cursorGlow { position:fixed; width:500px; height:500px; border-radius:50%; pointer-events:none; background:radial-gradient(circle,rgba(255,255,255,.16),transparent 70%); transform:translate(-50%,-50%); z-index:-1; }

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
      background: rgba(0,0,0,0.6);
      backdrop-filter: blur(12px);
      border-bottom: 1px solid rgba(255,255,255,0.08);
      z-index: 100;
    }
    .logo { font-size:24px; font-weight:900; letter-spacing:3px; background:linear-gradient(135deg,#fff,#aaa); -webkit-background-clip:text; -webkit-text-fill-color:transparent; }
    .nav-links { display:flex; gap:40px; align-items:center; }
    .nav-links a { color:rgba(255,255,255,0.7); text-decoration:none; font-weight:500; font-size:16px; transition:.3s; position:relative; }
    .nav-links a::after { content:''; position:absolute; bottom:-4px; left:0; width:0; height:2px; background:#fff; transition:.3s; }
    .nav-links a:hover { color:#fff; }
    .nav-links a:hover::after { width:100%; }
    .nav-links .auth-link { cursor:pointer; }

    /* ---------- Хедер ---------- */
    header { height:100vh; display:flex; justify-content:center; align-items:center; flex-direction:column; text-align:center; position:relative; padding-top:80px; }
    h1 { font-size:80px; font-weight:900; letter-spacing:8px; animation:glitch 5s infinite; }
    .subtitle { margin-top:20px; opacity:.75; font-size:20px; }
    .btn-primary { margin-top:50px; padding:20px 60px; font-size:20px; border:none; cursor:pointer; background:#fff; color:#000; font-weight:700; border-radius:50px; transition:.3s; }
    .btn-primary:hover { transform:scale(1.1); box-shadow:0 0 80px rgba(255,255,255,0.7); }

    /* ---------- Секции ---------- */
    section { padding:120px 10% 80px; }
    .section-title { font-size:50px; font-weight:900; margin-bottom:50px; text-align:center; }

    /* ---------- Тарифы ---------- */
    .tariff-container { max-width:900px; margin:0 auto; background:rgba(255,255,255,0.04); backdrop-filter:blur(16px); border:1px solid rgba(255,255,255,0.1); border-radius:40px; padding:50px 40px; }
    .days-selector { display:flex; flex-wrap:wrap; justify-content:center; gap:14px; margin-bottom:40px; }
    .day-btn { padding:14px 32px; border-radius:60px; border:1px solid rgba(255,255,255,0.2); background:transparent; color:rgba(255,255,255,0.7); font-weight:700; font-size:16px; cursor:pointer; transition:.3s; }
    .day-btn:hover { border-color:#fff; color:#fff; background:rgba(255,255,255,0.05); }
    .day-btn.active { background:#fff; color:#000; border-color:#fff; box-shadow:0 0 30px rgba(255,255,255,0.3); }
    .price-display { text-align:center; font-size:64px; font-weight:900; margin:30px 0 40px; letter-spacing:2px; }
    .price-display small { font-size:24px; font-weight:300; opacity:.6; }

    .payment-methods { display:flex; justify-content:center; margin-bottom:40px; }
    .payment-method { display:inline-flex; align-items:center; gap:14px; padding:18px 44px; border-radius:60px; border:1px solid rgba(255,255,255,0.2); background:rgba(255,255,255,0.05); color:#fff; font-weight:700; font-size:22px; text-decoration:none; transition:.3s; cursor:pointer; user-select:none; }
    .payment-method:hover { transform:scale(1.03); border-color:rgba(255,255,255,0.5); background:rgba(255,255,255,0.08); }
    .payment-method.active { border-color:#fff; background:rgba(255,255,255,0.15); box-shadow:0 0 50px rgba(255,255,255,0.2); transform:scale(1.05); }
    .payment-method i { font-size:36px; color:#ffb347; }

    .btn-buy { display:block; width:100%; max-width:320px; margin:0 auto; padding:18px 40px; font-size:22px; font-weight:700; border:none; border-radius:60px; background:#444; color:#888; cursor:not-allowed; transition:.3s; pointer-events:none; opacity:.6; }
    .btn-buy.active { background:linear-gradient(135deg,#fff,#ccc); color:#000; cursor:pointer; pointer-events:auto; opacity:1; }
    .btn-buy.active:hover { transform:scale(1.04); box-shadow:0 0 60px rgba(255,255,255,0.3); }

    /* ---------- Поддержка ---------- */
    .support-wrapper { display:flex; flex-direction:column; align-items:center; gap:30px; }
    .support-wrapper p { font-size:20px; opacity:.7; max-width:500px; text-align:center; }
    .discord-link { display:inline-flex; align-items:center; gap:16px; padding:18px 44px; border-radius:60px; background:#5865f2; color:#fff; font-size:24px; font-weight:700; text-decoration:none; transition:.3s; box-shadow:0 0 40px rgba(88,101,242,0.3); }
    .discord-link i { font-size:40px; }
    .discord-link:hover { transform:scale(1.08); box-shadow:0 0 80px rgba(88,101,242,0.6); }

    /* ---------- Футер ---------- */
    footer { padding:80px 10%; text-align:center; opacity:.6; border-top:1px solid rgba(255,255,255,0.05); font-weight:300; letter-spacing:1px; }

    /* ---------- Модальное окно ---------- */
    .modal-overlay {
      display: none;
      position: fixed;
      inset: 0;
      background: rgba(0,0,0,0.7);
      backdrop-filter: blur(8px);
      justify-content: center;
      align-items: center;
      z-index: 999;
    }
    .modal-overlay.show { display:flex; }
    .modal {
      background: #1a1a1a;
      border: 1px solid rgba(255,255,255,0.15);
      border-radius: 40px;
      padding: 40px;
      max-width: 440px;
      width: 90%;
      text-align: center;
      max-height: 90vh;
      overflow-y: auto;
    }
    .modal .tabs { display:flex; gap:20px; justify-content:center; margin-bottom:30px; }
    .modal .tabs span { font-size:20px; font-weight:700; opacity:.5; cursor:pointer; transition:.3s; }
    .modal .tabs span.active { opacity:1; border-bottom:2px solid #fff; padding-bottom:6px; }
    .modal form { display:flex; flex-direction:column; gap:16px; }
    .modal form input { padding:14px 20px; border-radius:30px; border:1px solid rgba(255,255,255,0.15); background:rgba(255,255,255,0.05); color:#fff; font-size:16px; outline:none; transition:.3s; }
    .modal form input:focus { border-color:#fff; }
    .modal form button { padding:14px; border-radius:30px; border:none; background:#fff; color:#000; font-weight:700; font-size:18px; cursor:pointer; transition:.3s; }
    .modal form button:hover { transform:scale(1.02); box-shadow:0 0 30px rgba(255,255,255,0.3); }
    .modal .error { color:#ff6b6b; font-size:14px; margin-top: -10px; }

    /* ---------- Кабинет ---------- */
    .cabinet {
      display: none;
      padding: 140px 10% 80px;
      max-width: 700px;
      margin: 0 auto;
    }
    .cabinet.show { display:block; }
    .cabinet .info-block { background:rgba(255,255,255,0.04); backdrop-filter:blur(16px); border:1px solid rgba(255,255,255,0.1); border-radius:30px; padding:40px; margin-bottom:30px; }
    .cabinet .info-block .row { display:flex; justify-content:space-between; padding:14px 0; border-bottom:1px solid rgba(255,255,255,0.05); }
    .cabinet .info-block .row:last-child { border-bottom:none; }
    .cabinet .info-block .label { opacity:.6; font-weight:300; }
    .cabinet .info-block .value { font-weight:500; }
    .cabinet .info-block .value .masked-email { cursor:help; border-bottom:1px dashed rgba(255,255,255,0.3); }
    .cabinet .actions { display:flex; flex-wrap:wrap; gap:16px; justify-content:center; margin-top:20px; }
    .cabinet .actions button { padding:14px 32px; border-radius:60px; border:none; font-weight:700; font-size:16px; cursor:pointer; transition:.3s; background:rgba(255,255,255,0.1); color:#fff; border:1px solid rgba(255,255,255,0.2); }
    .cabinet .actions button:hover { background:#fff; color:#000; transform:scale(1.05); }
    .cabinet .actions .btn-buy-client { background:linear-gradient(135deg,#fff,#ccc); color:#000; }
    .cabinet .actions .btn-buy-client:hover { box-shadow:0 0 40px rgba(255,255,255,0.3); }
    .cabinet .actions .btn-logout { background:#ff6b6b; border-color:#ff6b6b; }
    .cabinet .actions .btn-logout:hover { background:#ff4757; color:#fff; }

    /* ---------- Анимации ---------- */
    @keyframes glitch { 0%,100% { text-shadow:0 0 10px #fff,0 0 30px #fff; } 20% { transform:translateX(2px); } 21% { transform:translateX(-2px); } 40% { filter:blur(2px); } 60% { transform:rotate(.3deg); } 80% { letter-spacing:14px; } }
    .floating { position:absolute; border:1px solid rgba(255,255,255,0.12); border-radius:50%; animation:float linear infinite; pointer-events:none; }
    @keyframes float { from { transform:translateY(120vh) rotate(0); } to { transform:translateY(-150vh) rotate(360deg); } }

    /* ---------- Адаптивность ---------- */
    @media (max-width:768px) {
      nav { padding:16px 6%; flex-wrap:wrap; gap:12px; }
      .nav-links { gap:20px; }
      h1 { font-size:48px; letter-spacing:4px; }
      .subtitle { font-size:16px; }
      .btn-primary { padding:16px 40px; font-size:16px; }
      section { padding:80px 6% 60px; }
      .section-title { font-size:34px; }
      .tariff-container { padding:30px 20px; }
      .price-display { font-size:44px; }
      .day-btn { padding:10px 20px; font-size:14px; }
      .payment-method { padding:14px 28px; font-size:18px; }
      .payment-method i { font-size:28px; }
      .discord-link { font-size:18px; padding:14px 30px; }
      .discord-link i { font-size:30px; }
      footer { padding:60px 6%; font-size:14px; }
      .cabinet { padding:120px 6% 60px; }
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
      <span class="auth-link" id="authLink">Войти</span>
    </div>
  </nav>

  <!-- Основной контент (скрывается при открытом кабинете) -->
  <div id="mainContent">
    <header>
      <h1>PODIUM.GG</h1>
      <div class="subtitle">PREMIUM DLC EXPERIENCE</div>
      <button class="btn-primary" onclick="document.getElementById('plans').scrollIntoView({behavior:'smooth'})">
        ПОСМОТРЕТЬ ТАРИФЫ
      </button>
    </header>

    <section id="plans">
      <h2 class="section-title">Выберите тариф</h2>
      <div class="tariff-container">
        <div class="days-selector" id="daysSelector">
          <button class="day-btn" data-days="1" data-price="99">1 день</button>
          <button class="day-btn" data-days="3" data-price="159">3 дня</button>
          <button class="day-btn" data-days="7" data-price="229">7 дней</button>
          <button class="day-btn" data-days="14" data-price="349">14 дней</button>
          <button class="day-btn active" data-days="30" data-price="549">30 дней</button>
        </div>
        <div class="price-display" id="priceDisplay">549 <small>₽</small></div>
        <div class="payment-methods">
          <div class="payment-method" id="funpayMethod">
            <i class="fas fa-coins"></i> FunPay
          </div>
        </div>
        <button class="btn-buy" id="buyBtn" disabled>Оплатить</button>
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
      <button id="extendSubscription">Продлить подписку (демо)</button>
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
      <!-- Форма входа -->
      <form id="loginForm">
        <input type="text" placeholder="Логин" id="loginLogin" required />
        <input type="password" placeholder="Пароль" id="loginPassword" required />
        <div class="error" id="loginError"></div>
        <button type="submit">Войти</button>
      </form>
      <!-- Форма регистрации -->
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

  <script>
    // ============================================================
    // 1. Свечение, звёзды, плавающие круги (без изменений)
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

    // ============================================================
    // 2. Логика тарифов и оплаты
    // ============================================================
    const daysBtns = document.querySelectorAll('.day-btn');
    const priceDisplay = document.getElementById('priceDisplay');
    const buyBtn = document.getElementById('buyBtn');
    const funpayMethod = document.getElementById('funpayMethod');
    let selectedDays = 30, selectedPrice = 549, paymentSelected = false;

    daysBtns.forEach(btn => {
      btn.addEventListener('click', () => {
        daysBtns.forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
        selectedDays = parseInt(btn.dataset.days);
        selectedPrice = parseInt(btn.dataset.price);
        priceDisplay.innerHTML = `${selectedPrice} <small>₽</small>`;
      });
    });

    funpayMethod.addEventListener('click', () => {
      funpayMethod.classList.toggle('active');
      paymentSelected = funpayMethod.classList.contains('active');
      if (paymentSelected) {
        buyBtn.disabled = false;
        buyBtn.classList.add('active');
      } else {
        buyBtn.disabled = true;
        buyBtn.classList.remove('active');
      }
    });

    buyBtn.addEventListener('click', () => {
      if (paymentSelected) {
        window.open('https://funpay.com/users/20739931/', '_blank');
      }
    });

    // ============================================================
    // 3. Система регистрации/входа/кабинета (исправленная)
    // ============================================================
    let users = JSON.parse(localStorage.getItem('podium_users')) || [];
    let currentUser = JSON.parse(localStorage.getItem('podium_currentUser')) || null;

    // DOM-элементы
    const authLink = document.getElementById('authLink');
    const authModal = document.getElementById('authModal');
    const loginForm = document.getElementById('loginForm');
    const registerForm = document.getElementById('registerForm');
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
    const extendSubBtn = document.getElementById('extendSubscription');

    // Маскировка почты
    function maskEmail(email) {
      if (!email) return '***';
      const at = email.indexOf('@');
      if (at <= 1) return '***' + email.substring(at);
      return email[0] + '***' + email.substring(at);
    }

    // Заполнение данных в кабинете (без изменения видимости)
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

    // Показать кабинет
    function showCabinet() {
      if (!currentUser) return;
      mainContent.style.display = 'none';
      cabinet.classList.add('show');
      fillCabinetData();
    }

    // Скрыть кабинет (показать основной контент)
    function hideCabinet() {
      mainContent.style.display = 'block';
      cabinet.classList.remove('show');
    }

    // Обновить интерфейс (ссылка в навигации и видимость)
    function updateUI() {
      if (currentUser) {
        authLink.textContent = 'Кабинет';
        // Если кабинет не показан и мы не находимся в режиме "покупки клиента", то показываем кабинет.
        // Но мы не знаем, скрыт ли он вручную. Проверим, если mainContent скрыт и cabinet не показан,
        // значит мы в состоянии "скрыто" - тогда показываем кабинет.
        // Однако проще: при входе всегда показываем кабинет.
        // Но чтобы не перекрывать ручное скрытие (когда мы нажали "Купить клиент"), мы не будем автоматически показывать.
        // Лучше оставить управление видимостью только через showCabinet/hideCabinet.
        // При входе вызываем showCabinet, при выходе hideCabinet.
        // При загрузке, если currentUser есть, тоже showCabinet.
      } else {
        authLink.textContent = 'Войти';
        hideCabinet();
      }
    }

    // Сохранение
    function saveUsers() {
      localStorage.setItem('podium_users', JSON.stringify(users));
    }
    function saveCurrentUser() {
      localStorage.setItem('podium_currentUser', JSON.stringify(currentUser));
    }
    function generateUid() {
      return 'UID-' + Math.random().toString(36).substring(2, 10).toUpperCase();
    }

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

    // Клик по ссылке входа/кабинета
    authLink.addEventListener('click', (e) => {
      e.preventDefault();
      if (currentUser) {
        // Если пользователь залогинен – открываем кабинет
        showCabinet();
        return;
      }
      // Иначе открываем модалку
      authModal.classList.add('show');
      loginForm.style.display = 'flex';
      registerForm.style.display = 'none';
      tabs.forEach(t => t.classList.remove('active'));
      document.querySelector('.tabs span[data-tab="login"]').classList.add('active');
      loginError.textContent = '';
      regError.textContent = '';
    });

    // Закрыть модалку по клику вне
    authModal.addEventListener('click', (e) => {
      if (e.target === authModal) {
        authModal.classList.remove('show');
      }
    });

    // Регистрация
    registerForm.addEventListener('submit', (e) => {
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
        subscription: { expires: null }
      };
      users.push(newUser);
      saveUsers();
      currentUser = newUser;
      saveCurrentUser();
      authModal.classList.remove('show');
      updateUI();
      showCabinet(); // показываем кабинет после регистрации
    });

    // Вход
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
      currentUser = user;
      saveCurrentUser();
      authModal.classList.remove('show');
      updateUI();
      showCabinet();
    });

    // Выход
    logoutBtn.addEventListener('click', () => {
      currentUser = null;
      localStorage.removeItem('podium_currentUser');
      updateUI();
      hideCabinet();
      window.scrollTo({top:0, behavior:'smooth'});
    });

    // Кнопка "Скачать лаунчер"
    downloadLauncher.addEventListener('click', () => {
      alert('Скачивание лаунчера начнется... (демо)');
    });

    // Кнопка "Купить клиент" – скрываем кабинет и скроллим к тарифам
    buyClientBtn.addEventListener('click', () => {
      hideCabinet();
      document.getElementById('plans').scrollIntoView({behavior:'smooth'});
    });

    // Кнопка "Продлить подписку" (демо)
    extendSubBtn.addEventListener('click', () => {
      if (!currentUser) return;
      let expires = currentUser.subscription && currentUser.subscription.expires ? new Date(currentUser.subscription.expires) : new Date();
      if (expires < new Date()) expires = new Date();
      expires.setDate(expires.getDate() + 30);
      currentUser.subscription = { expires: expires.toISOString() };
      const idx = users.findIndex(u => u.uid === currentUser.uid);
      if (idx !== -1) {
        users[idx].subscription = currentUser.subscription;
        saveUsers();
      }
      saveCurrentUser();
      fillCabinetData(); // обновляем данные без переключения видимости
      alert('Подписка продлена на 30 дней! (демо)');
    });

    // ============================================================
    // 4. Инициализация при загрузке
    // ============================================================
    if (currentUser) {
      updateUI();
      showCabinet();
    } else {
      updateUI();
      hideCabinet();
    }

    console.log('✅ Все системы готовы. Теперь кабинет открывается по клику на "Кабинет" после скрытия.');
  </script>
</body>
</html>
