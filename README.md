# Waiter.test<!DOCTYPE html>

<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Тест для официантов — Премиум-стандарты обслуживания</title>
<style>
  /* ===== RESET & BASE ===== */
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
  :root {
    --bg: #f5f5f7;
    --surface: #ffffff;
    --accent: #1d1d1f;
    --accent2: #c8a96e;
    --text: #1d1d1f;
    --muted: #86868b;
    --border: #d2d2d7;
    --correct: #1a7f3c;
    --wrong: #c1121f;
    --radius: 18px;
    --shadow: 0 4px 40px rgba(0,0,0,.10);
    --spring: cubic-bezier(0.34, 1.56, 0.64, 1);
    --ease-out: cubic-bezier(0.25, 0.46, 0.45, 0.94);
  }
  html { -webkit-font-smoothing: antialiased; }
  body {
    font-family: -apple-system, 'SF Pro Display', 'Segoe UI', system-ui, sans-serif;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 24px 16px;
    overflow-x: hidden;
  }
  .container { width: 100%; max-width: 640px; }

/* ===== CARD ===== */
.card {
background: var(–surface);
border-radius: var(–radius);
box-shadow: var(–shadow);
padding: 44px 48px;
position: relative;
overflow: hidden;
}
/* Subtle grain texture */
.card::before {
content: ‘’;
position: absolute; inset: 0;
background-image: url(“data:image/svg+xml,%3Csvg viewBox=‘0 0 200 200’ xmlns=‘http://www.w3.org/2000/svg’%3E%3Cfilter id=‘n’%3E%3CfeTurbulence type=‘fractalNoise’ baseFrequency=‘0.9’ numOctaves=‘4’ stitchTiles=‘stitch’/%3E%3C/filter%3E%3Crect width=‘100%25’ height=‘100%25’ filter=‘url(%23n)’ opacity=‘0.025’/%3E%3C/svg%3E”);
pointer-events: none; z-index: 0; border-radius: var(–radius);
}
.card > * { position: relative; z-index: 1; }

/* ===== SCREEN TRANSITIONS ===== */
.screen { display: none; opacity: 0; }
.screen.active { display: block; }
.screen.anim-in {
animation: screenIn .5s cubic-bezier(0.25,0.46,0.45,0.94) both;
}
.screen.anim-out {
animation: screenOut .28s cubic-bezier(0.25,0.46,0.45,0.94) both;
}
@keyframes screenIn {
from { opacity: 0; transform: translateY(24px) scale(0.98); }
to   { opacity: 1; transform: translateY(0) scale(1); }
}
@keyframes screenOut {
from { opacity: 1; transform: translateY(0) scale(1); }
to   { opacity: 0; transform: translateY(-14px) scale(0.97); }
}

/* ===== CHILD STAGGER — triggered by JS adding .did-enter to screen ===== */
.screen .anim-child { opacity: 0; transform: translateY(18px); transition: none; }
.screen.did-enter .anim-child {
animation: fadeSlideUp .55s cubic-bezier(0.25,0.46,0.45,0.94) both;
}
.screen.did-enter .anim-child:nth-child(1) { animation-delay: .05s; }
.screen.did-enter .anim-child:nth-child(2) { animation-delay: .12s; }
.screen.did-enter .anim-child:nth-child(3) { animation-delay: .18s; }
.screen.did-enter .anim-child:nth-child(4) { animation-delay: .24s; }
.screen.did-enter .anim-child:nth-child(5) { animation-delay: .30s; }
.screen.did-enter .anim-child:nth-child(6) { animation-delay: .36s; }
@keyframes fadeSlideUp {
from { opacity: 0; transform: translateY(18px); }
to   { opacity: 1; transform: translateY(0); }
}

/* ===== LOGO ===== */
.logo {
display: flex; align-items: center; gap: 12px;
margin-bottom: 36px;
}
.logo-icon {
width: 40px; height: 40px;
background: var(–accent);
border-radius: 10px;
display: flex; align-items: center; justify-content: center;
box-shadow: 0 2px 12px rgba(0,0,0,.18);
}
.logo-icon svg { width: 20px; height: 20px; fill: var(–accent2); }
.logo-text { font-size: 12px; font-weight: 600; letter-spacing: .08em; text-transform: uppercase; color: var(–muted); }

/* ===== TYPOGRAPHY ===== */
h1 {
font-size: 28px; font-weight: 700; line-height: 1.2; margin-bottom: 8px;
letter-spacing: -.5px;
}
.subtitle {
font-size: 15px; color: var(–muted); margin-bottom: 36px; line-height: 1.5;
}

/* ===== FORM ===== */
.form-group { }
label {
display: block; font-size: 11px; font-weight: 600;
letter-spacing: .06em; text-transform: uppercase;
color: var(–muted); margin-bottom: 7px;
}
input[type=“text”] {
width: 100%; padding: 13px 16px;
border: 1.5px solid var(–border);
border-radius: 12px; font-size: 16px;
background: var(–bg); color: var(–text);
outline: none;
transition: border-color .25s, box-shadow .25s, transform .2s var(–spring);
margin-bottom: 18px;
-webkit-appearance: none;
}
input[type=“text”]:focus {
border-color: var(–accent2);
box-shadow: 0 0 0 4px rgba(200,169,110,.15);
transform: scale(1.005);
}

/* ===== BUTTONS ===== */
.btn-wrap { }
.btn {
display: inline-flex; align-items: center; justify-content: center; gap: 8px;
background: var(–accent); color: #fff;
border: none; border-radius: 14px;
font-size: 15px; font-weight: 600;
padding: 14px 28px; cursor: pointer;
width: 100%;
transition: background .2s, transform .18s var(–spring), box-shadow .2s;
letter-spacing: -.1px;
-webkit-tap-highlight-color: transparent;
}
.btn:hover { background: #333; box-shadow: 0 6px 20px rgba(0,0,0,.18); transform: translateY(-1px); }
.btn:active { transform: scale(.97) translateY(0); box-shadow: none; }
.btn.secondary {
background: rgba(0,0,0,.04); color: var(–text);
border: 1.5px solid var(–border);
margin-top: 10px;
box-shadow: none;
}
.btn.secondary:hover { background: rgba(0,0,0,.07); transform: translateY(-1px); box-shadow: 0 4px 14px rgba(0,0,0,.08); }
.btn.gold {
background: linear-gradient(135deg, #d4a843 0%, #c8a96e 50%, #b8924a 100%);
color: #fff;
box-shadow: 0 4px 20px rgba(200,169,110,.35);
}
.btn.gold:hover { box-shadow: 0 8px 28px rgba(200,169,110,.45); transform: translateY(-2px); }

/* ===== ERROR ===== */
#login-error {
font-size: 13px; color: var(–wrong); margin-bottom: 14px; display: none;
animation: shake .4s var(–spring);
}
@keyframes shake {
0%,100% { transform: translateX(0); }
20%      { transform: translateX(-6px); }
40%      { transform: translateX(6px); }
60%      { transform: translateX(-4px); }
80%      { transform: translateX(4px); }
}

/* ===== PROGRESS ===== */
.progress-wrap { margin-bottom: 32px; }
.progress-meta { display: flex; justify-content: space-between; font-size: 13px; color: var(–muted); margin-bottom: 10px; font-weight: 500; }
.progress-bar-bg {
height: 5px; background: var(–border); border-radius: 99px; overflow: hidden;
position: relative;
}
.progress-bar-fill {
height: 100%;
background: linear-gradient(90deg, #c8a96e, #e8c97e, #c8a96e);
background-size: 200% 100%;
border-radius: 99px;
transition: width .6s var(–spring);
animation: shimmer 2.5s infinite linear;
}
@keyframes shimmer { 0% { background-position: 200% 0; } 100% { background-position: -200% 0; } }

/* ===== TIMER ===== */
.timer-wrap {
display: flex; align-items: center; gap: 6px;
font-size: 13px; font-weight: 600; color: var(–muted);
transition: color .3s;
}
.timer-wrap.danger { color: var(–wrong); }
.timer-dot {
width: 7px; height: 7px; border-radius: 50%;
background: currentColor;
animation: pulse 1.2s ease-in-out infinite;
}
@keyframes pulse { 0%,100%{opacity:1;transform:scale(1)} 50%{opacity:.3;transform:scale(.7)} }
.timer-wrap.danger .timer-dot { animation: pulseFast .6s ease-in-out infinite; }
@keyframes pulseFast { 0%,100%{opacity:1;transform:scale(1.2)} 50%{opacity:.2;transform:scale(.6)} }

/* ===== QUESTION ===== */
.q-num {
font-size: 11px; font-weight: 700; letter-spacing: .1em;
text-transform: uppercase; color: var(–accent2);
margin-bottom: 14px; display: block;
}
.q-text {
font-size: 18px; font-weight: 600; line-height: 1.55;
margin-bottom: 24px; letter-spacing: -.2px;
}

/* ===== OPTIONS ===== */
.options { display: flex; flex-direction: column; gap: 10px; }
.option {
border: 1.5px solid var(–border);
border-radius: 14px; padding: 15px 18px;
font-size: 15px; cursor: pointer;
display: flex; align-items: center; gap: 14px;
text-align: left;
background: var(–surface);
line-height: 1.4;
transition: border-color .2s, background .2s, transform .2s var(–spring), box-shadow .2s;
-webkit-tap-highlight-color: transparent;
/* staggered entrance — set via JS */
opacity: 0;
transform: translateY(14px);
}
.option.visible {
animation: optionIn .45s var(–spring) both;
}
@keyframes optionIn {
from { opacity: 0; transform: translateY(16px) scale(0.97); }
to   { opacity: 1; transform: translateY(0) scale(1); }
}
.option:hover:not(.disabled) {
border-color: var(–accent2);
background: #fdf8f0;
transform: translateY(-2px) scale(1.005);
box-shadow: 0 6px 20px rgba(200,169,110,.14);
}
.option:active:not(.disabled) { transform: scale(.98); }
.option.correct {
border-color: var(–correct) !important;
background: #e6f9ee !important;
color: var(–correct) !important;
opacity: 1 !important;
animation: correctPop .5s cubic-bezier(0.34,1.56,0.64,1) both;
}
@keyframes correctPop {
0%   { opacity: 1; transform: scale(1); background: #ffffff; }
40%  { opacity: 1; transform: scale(1.04); }
100% { opacity: 1; transform: scale(1); }
}
.option.wrong {
border-color: var(–wrong) !important;
background: #fff0f1 !important;
color: var(–wrong) !important;
opacity: 1 !important;
animation: wrongShake .45s cubic-bezier(0.34,1.56,0.64,1) both;
}
@keyframes wrongShake {
0%   { opacity: 1; transform: translateX(0); }
20%  { opacity: 1; transform: translateX(-7px); }
50%  { opacity: 1; transform: translateX(6px); }
75%  { opacity: 1; transform: translateX(-4px); }
100% { opacity: 1; transform: translateX(0); }
}
.option.disabled { cursor: default; }
.option-letter {
width: 28px; height: 28px; flex-shrink: 0;
border: 1.5px solid currentColor;
border-radius: 50%; display: flex; align-items: center; justify-content: center;
font-size: 12px; font-weight: 700; opacity: .45;
transition: all .2s;
}
.option.correct .option-letter {
opacity: 1; background: var(–correct); color: #fff; border-color: var(–correct);
}
.option.wrong .option-letter {
opacity: 1; background: var(–wrong); color: #fff; border-color: var(–wrong);
}
.option:hover:not(.disabled) .option-letter { opacity: .8; }

/* ===== RESULT ===== */
.result-header { text-align: center; margin-bottom: 32px; padding-top: 8px; }
.result-trophy {
font-size: 52px; margin-bottom: 12px; display: block;
animation: trophyBounce .7s var(–spring) .1s both;
}
@keyframes trophyBounce {
from { opacity: 0; transform: scale(.3) rotate(-20deg); }
60%  { transform: scale(1.15) rotate(5deg); }
to   { opacity: 1; transform: scale(1) rotate(0); }
}
.result-score {
font-size: 72px; font-weight: 800; line-height: 1;
color: var(–accent); margin-bottom: 4px;
letter-spacing: -3px;
animation: countUp .8s var(–ease-out) .2s both;
}
@keyframes countUp {
from { opacity: 0; transform: scale(.6) translateY(20px); }
to   { opacity: 1; transform: scale(1) translateY(0); }
}
.result-score span { font-size: 30px; font-weight: 400; color: var(–muted); letter-spacing: -1px; }
.result-pct {
font-size: 22px; color: var(–muted); font-weight: 500; margin-bottom: 4px;
}
.result-grade {
display: inline-block;
padding: 7px 20px; border-radius: 99px;
font-size: 14px; font-weight: 700;
margin: 12px 0 0;
}
@keyframes gradePop {
from { opacity: 0; transform: scale(.7); }
to   { opacity: 1; transform: scale(1); }
}
.grade-excellent { background: #e3f9ec; color: #1a7f3c; }
.grade-good      { background: #e4f0fd; color: #1565c0; }
.grade-satisfactory { background: #fff8e1; color: #9a6f00; }
.grade-fail      { background: #fff0f1; color: var(–wrong); }

.result-info {
background: var(–bg); border-radius: 14px; padding: 20px 22px; margin: 28px 0;
}
.result-row {
display: flex; justify-content: space-between; align-items: center;
font-size: 14px; padding: 8px 0; border-bottom: 1px solid var(–border);
transition: background .2s;
}
.result-row:last-child { border-bottom: none; }
.result-row strong { font-weight: 600; }

.btn-group {
display: flex; flex-direction: column; gap: 10px;
}

/* ===== RESPONSIVE ===== */
@media(max-width: 520px) {
.card { padding: 32px 22px; }
h1 { font-size: 24px; }
.result-score { font-size: 56px; }
.q-text { font-size: 16px; }
}
</style>

</head>
<body>
<div class="container">
  <div class="card">

```
<!-- ===================== SCREEN 1: LOGIN ===================== -->
<div id="screen-login" class="screen">
  <div class="logo anim-child">
    <div class="logo-icon">
      <svg viewBox="0 0 24 24"><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-2 14.5v-9l6 4.5-6 4.5z"/></svg>
    </div>
    <span class="logo-text">Рестобар · Обучение</span>
  </div>
  <h1 class="anim-child">Тест для официантов</h1>
  <p class="subtitle anim-child">Премиум-стандарты обслуживания. Время на прохождение — 5 минут.</p>
  <div class="form-group anim-child">
    <label for="inp-name">Имя и фамилия</label>
    <input type="text" id="inp-name" placeholder="Иван Петров" autocomplete="name">
    <label for="inp-pos">Должность</label>
    <input type="text" id="inp-pos" placeholder="Официант">
    <div id="login-error">Пожалуйста, заполните оба поля.</div>
  </div>
  <div class="btn-wrap anim-child">
    <button class="btn" onclick="startTest()">Начать тест</button>
  </div>
</div>

<!-- ===================== SCREEN 2: QUESTION ===================== -->
<div id="screen-question" class="screen">
  <div class="progress-wrap">
    <div class="progress-meta">
      <span id="q-counter">Вопрос 1 из 20</span>
      <div class="timer-wrap" id="timer-wrap">
        <div class="timer-dot"></div>
        <span id="timer-display">05:00</span>
      </div>
    </div>
    <div class="progress-bar-bg">
      <div class="progress-bar-fill" id="progress-fill" style="width:0%"></div>
    </div>
  </div>
  <span class="q-num" id="q-num">Вопрос 1</span>
  <p class="q-text" id="q-text"></p>
  <div class="options" id="options-wrap"></div>
</div>

<!-- ===================== SCREEN 3: RESULT ===================== -->
<div id="screen-result" class="screen">
  <div class="logo anim-child">
    <div class="logo-icon">
      <svg viewBox="0 0 24 24"><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-2 14.5v-9l6 4.5-6 4.5z"/></svg>
    </div>
    <span class="logo-text">Результат теста</span>
  </div>
  <div class="result-header anim-child">
    <span class="result-trophy" id="res-trophy">🏆</span>
    <div class="result-score" id="res-score">0<span>/20</span></div>
    <div class="result-pct" id="res-pct">0%</div>
    <div class="result-grade" id="res-grade">-</div>
  </div>
  <div class="result-info anim-child">
    <div class="result-row"><span>Сотрудник</span><strong id="res-name">-</strong></div>
    <div class="result-row"><span>Должность</span><strong id="res-pos">-</strong></div>
    <div class="result-row"><span>Дата и время</span><strong id="res-date">-</strong></div>
    <div class="result-row"><span>Правильных ответов</span><strong id="res-correct">-</strong></div>
    <div class="result-row"><span>Затрачено времени</span><strong id="res-time">-</strong></div>
  </div>
  <div class="btn-group anim-child">
    <button class="btn gold" onclick="downloadPDF()">Скачать результат в PDF</button>
    <button class="btn secondary" onclick="sendEmail()">Отправить администратору на email</button>
    <button class="btn secondary" onclick="restartTest()">Пройти тест заново</button>
  </div>
</div>
```

  </div>
</div>

<script>
/* ============================================================
   МАССИВ ВОПРОСОВ — администратор может изменять здесь
   Формат каждого вопроса:
   {
     q: "Текст вопроса",
     options: ["вариант1","вариант2","вариант3","вариант4"],
     answer: 0  // индекс правильного варианта в массиве options
   }
   ============================================================ */
const QUESTIONS_BANK = [
  {
    q: "Сколько ключевых принципов лежит в основе философии премиум-обслуживания в рестобаре?",
    options: ["Пять", "Четыре", "Два", "Три"],
    answer: 3
  },
  {
    q: "Что означает концепция «Невидимый сервис»?",
    options: [
      "Официант наблюдает за гостем издалека и подходит только по жёсткому регламенту",
      "Официант работает преимущественно в зоне, не видимой гостям",
      "Официант присутствует рядом, когда нужен, и ненавязчиво отступает, когда гость хочет уединения",
      "Обслуживание выполняется только при прямом запросе гостя, без инициативы"
    ],
    answer: 2
  },
  {
    q: "За сколько минут до начала смены официант должен прибыть на работу?",
    options: ["За 45 минут", "За 15 минут", "За 30 минут", "За 20 минут"],
    answer: 2
  },
  {
    q: "Какой из перечисленных предметов официанту РАЗРЕШЕНО носить во время работы?",
    options: [
      "Тонкий браслет на запястье",
      "Обручальное кольцо",
      "Серьги-кольца небольшого размера",
      "Кольцо с камнем на указательном пальце"
    ],
    answer: 1
  },
  {
    q: "Гость не должен ждать приветствия дольше чем...",
    options: ["1 минуты", "45 секунд", "15 секунд", "30 секунд"],
    answer: 3
  },
  {
    q: "В каком порядке подается меню гостям за столом?",
    options: [
      "Сначала хозяину стола, затем дамам по старшинству, затем мужчинам",
      "Сначала дамам по старшинству, затем мужчинам, последним — хозяину стола",
      "Сначала старшему мужчине, затем дамам, затем остальным мужчинам",
      "Одновременно всем гостям, начиная с ближайшего к официанту"
    ],
    answer: 1
  },
  {
    q: "С какой стороны официант подает меню гостю?",
    options: [
      "С левой — чтобы не мешать правой руке гостя",
      "Со стороны, откуда удобнее подойти",
      "С правой стороны от гостя",
      "Меню кладется на стол перед гостем без подачи из рук"
    ],
    answer: 2
  },
  {
    q: "Сколько времени нужно дать гостям на изучение меню перед принятием заказа?",
    options: ["2–3 минуты", "15–20 минут", "5–10 минут", "Ровно 1 минуту"],
    answer: 2
  },
  {
    q: "Что означает правило «открытой руки» при обслуживании?",
    options: [
      "При подходе к столу держать руки открытыми, демонстрируя отсутствие предметов",
      "Подача и уборка выполняются ладонью, обращённой вверх, без захвата краёв посуды пальцами сверху",
      "Правая рука всегда остаётся свободной для быстрого реагирования на просьбы гостя",
      "Блюда подаются двумя руками одновременно для устойчивости"
    ],
    answer: 1
  },
  {
    q: "Как часто официант должен ненавязчиво проверять столик гостя?",
    options: ["Каждые 10–15 минут", "Каждые 2–3 минуты", "Каждые 5–7 минут", "Только когда гость подаёт знак"],
    answer: 2
  },
  {
    q: "Какова правильная высота шапки пены при наливании пива в бокал?",
    options: ["3–5 см", "0,5–1 см", "4–6 см", "1,5–2 см"],
    answer: 3
  },
  {
    q: "При какой температуре следует подавать светлое пиво?",
    options: ["10–12°C", "2–4°C", "6–8°C", "14–16°C"],
    answer: 2
  },
  {
    q: "Какой объем вина наливается хозяину стола для первоначальной дегустации?",
    options: ["50 мл", "20 мл", "75 мл", "30 мл"],
    answer: 3
  },
  {
    q: "На какую часть объема наполняется бокал красным вином?",
    options: ["На 1/2", "На 2/3", "На 1/3", "На 1/4"],
    answer: 2
  },
  {
    q: "Какова стандартная порция виски при подаче гостю?",
    options: ["40 мл", "60 мл", "30 мл", "50 мл"],
    answer: 3
  },
  {
    q: "При какой температуре подается кофе?",
    options: ["95–100°C", "75–80°C", "85–90°C", "70–75°C"],
    answer: 2
  },
  {
    q: "Что в классической последовательности подачи подается ПЕРЕД основным блюдом?",
    options: ["Сыры", "Десерт", "Горячие закуски и суп", "Дижестив"],
    answer: 2
  },
  {
    q: "Как правильно держать и переносить круглый поднос?",
    options: [
      "На ладони и предплечье левой руки, правая поддерживает дальний край",
      "На ладони и пальцах левой руки на уровне плеча, правая рука свободна или слегка придерживает край",
      "На двух руках на уровне груди — для максимальной устойчивости",
      "На ладони правой руки, левая рука свободна для открытия дверей"
    ],
    answer: 1
  },
  {
    q: "Где при сервировке стола размещается бокал для воды?",
    options: [
      "По центру над тарелкой, симметрично",
      "Слева вверху, над вилками",
      "В верхнем правом углу от тарелки, над кончиком ножа",
      "Справа от тарелки, на одной линии с приборами"
    ],
    answer: 2
  },
  {
    q: "Как правильно действовать при обслуживании нетрезвого гостя?",
    options: [
      "Продолжать обслуживание в обычном режиме, не акцентируя внимание",
      "Незаметно уведомить менеджера и полностью передать стол другому официанту",
      "Спокойно и уважительно прекратить подачу алкоголя, предложить воду, еду и помощь с транспортом",
      "Сразу предложить счёт и попросить покинуть заведение"
    ],
    answer: 2
  },
  {
    q: "Что из перечисленного ЗАПРЕЩЕНО при обслуживании VIP-гостя?",
    options: [
      "Проверять стол чаще, чем у других гостей",
      "Координировать обслуживание с менеджером смены",
      "Обсуждать личность гостя с коллегами или другими посетителями",
      "Выполнять нестандартные запросы гостя в рамках возможного"
    ],
    answer: 2
  },
  {
    q: "Как правильно реагировать на раздражённого или недовольного гостя?",
    options: [
      "Дать гостю высказаться, затем спокойно объяснить позицию заведения и предложить компромисс",
      "Незамедлительно согласиться со всем, пообещать скидку и позвать менеджера",
      "Сохранять спокойствие, выслушать до конца без возражений, признать неудобство и предложить конкретное решение",
      "Сразу эскалировать ситуацию на менеджера, самому не вступать в диалог"
    ],
    answer: 2
  },
  {
    q: "Какой полный список обязательных предметов официант должен иметь при себе во время смены?",
    options: [
      "Ручка, блокнот, зажигалка — три обязательных предмета",
      "Ручка (2 шт.), блокнот, штопор, зажигалка, белая салфетка",
      "Ручка, блокнот, штопор, телефон для связи с кухней",
      "Блокнот, штопор, открывалка, зажигалка, маркер для стаканов"
    ],
    answer: 1
  },
  {
    q: "Какую дистанцию следует соблюдать при общении с гостем у стола?",
    options: ["50–60 см", "110–130 см", "30–50 см", "70–100 см"],
    answer: 3
  },
  {
    q: "Как должна быть ориентирована тарелка при подаче блюда?",
    options: [
      "Логотип или основной элемент декора — на «6 часов», лицом к гостю",
      "Логотип или основной элемент декора — на «3 часа», вправо",
      "Логотип или основной элемент декора — на «12 часов», от гостя",
      "Ориентация тарелки не регламентируется стандартом"
    ],
    answer: 2
  },
  {
    q: "На каком расстоянии от края стола размещается подставная тарелка?",
    options: ["5 см от края", "Вплотную к краю стола", "10 см от края", "2 см от края"],
    answer: 3
  },
  {
    q: "Как правильно наливать вино гостям?",
    options: [
      "Держа бутылку за горлышко, этикеткой к себе, наливая через правое плечо гостя",
      "Держа за нижнюю часть бутылки, этикеткой к гостю; завершать лёгким поворотом бутылки",
      "Держа за среднюю часть бутылки двумя руками для устойчивости",
      "Наливать предварительно в декантер и разливать оттуда всем гостям"
    ],
    answer: 1
  },
  {
    q: "Когда следует приступать к уборке тарелок со стола?",
    options: [
      "Как только официант замечает, что большинство гостей закончили",
      "После того как каждый гость закончил есть — убирать поочерёдно",
      "Когда все гости за столом закончили еду — убирать одновременно",
      "Через 10 минут после подачи блюда, независимо от готовности гостей"
    ],
    answer: 2
  },
  {
    q: "В чём заключается роль официанта как «психолога»?",
    options: [
      "Умение работать с жалобами и разрешать конфликты между гостями",
      "Знание алкогольных напитков и их воздействия на организм человека",
      "Считывание настроения гостей, адаптация стиля обслуживания и создание комфортной атмосферы",
      "Способность определять платёжеспособность гостя и подбирать рекомендации по бюджету"
    ],
    answer: 2
  },
  {
    q: "Какое действие официанта при жалобе гостя на качество блюда является ПРАВИЛЬНЫМ первым шагом?",
    options: [
      "Немедленно забрать блюдо и принести замену без лишних слов",
      "Объяснить гостю технологию приготовления и почему блюдо выглядит именно так",
      "Внимательно выслушать жалобу, не перебивая, и принести извинения за доставленные неудобства",
      "Предложить скидку или комплимент ещё до выяснения сути претензии"
    ],
    answer: 2
  },
  {
    q: "Что из перечисленного относится к принципу «предвосхищения потребностей» гостя?",
    options: [
      "Спрашивать гостя каждые несколько минут, всё ли ему нравится",
      "Пополнить бокал с водой или заменить использованную салфетку до того, как гость об этом попросит",
      "Подробно рассказывать о каждом блюде и напитке при подаче",
      "Уточнять у гостя предпочтения по каждому параметру заказа заранее"
    ],
    answer: 1
  }
];

/* ============================================================
   НАСТРОЙКИ ТЕСТА — администратор может изменить здесь
   ============================================================ */
const TOTAL_QUESTIONS = 20;         // Количество вопросов в тесте (из банка)
const TIMER_SECONDS   = 5 * 60;     // Время теста в секундах (5 минут)
const ADMIN_EMAIL     = "admin@restobar.ru"; // Email администратора

// Инициализация при загрузке страницы
document.addEventListener("DOMContentLoaded", () => {
  // Скрыть все экраны явно
  document.querySelectorAll(".screen").forEach(s => { s.style.display = "none"; });
  // Показать стартовый экран с анимацией
  setTimeout(() => show("screen-login"), 80);
});
let state = {
  name: "",
  position: "",
  questions: [],   // перемешанные вопросы
  current: 0,
  score: 0,
  startTime: null,
  timerInterval: null,
  timerLeft: TIMER_SECONDS,
  finishTime: null
};

/* ============================================================
   УТИЛИТЫ
   ============================================================ */
function shuffle(arr) {
  const a = [...arr];
  for (let i = a.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [a[i], a[j]] = [a[j], a[i]];
  }
  return a;
}

function formatTime(sec) {
  const m = Math.floor(sec / 60).toString().padStart(2, "0");
  const s = (sec % 60).toString().padStart(2, "0");
  return `${m}:${s}`;
}

function formatDate(date) {
  return date.toLocaleString("ru-RU", {
    day: "2-digit", month: "2-digit", year: "numeric",
    hour: "2-digit", minute: "2-digit"
  });
}

function show(id) {
  const current = document.querySelector(".screen.active");

  const enter = (screenEl) => {
    // Сбросить did-enter чтобы анимации дочерних элементов повторились
    screenEl.classList.remove("did-enter", "anim-in");
    screenEl.style.display = "block";
    screenEl.classList.add("active");
    // Форс-рефлоу чтобы браузер зафиксировал начальное состояние
    void screenEl.offsetHeight;
    screenEl.classList.add("anim-in");
    // Небольшая задержка перед stagger детей
    setTimeout(() => screenEl.classList.add("did-enter"), 60);
  };

  if (current && current.id !== id) {
    current.classList.add("anim-out");
    current.addEventListener("animationend", function handler() {
      current.removeEventListener("animationend", handler);
      current.classList.remove("active", "anim-in", "anim-out", "did-enter");
      current.style.display = "none";
      enter(document.getElementById(id));
    }, { once: true });
  } else if (!current) {
    enter(document.getElementById(id));
  }
}

/* ============================================================
   ЛОГИКА ТЕСТА
   ============================================================ */
function startTest() {
  const name = document.getElementById("inp-name").value.trim();
  const pos  = document.getElementById("inp-pos").value.trim();
  if (!name || !pos) {
    const err = document.getElementById("login-error");
    err.style.display = "block";
    // Перезапустить анимацию shake
    err.style.animation = "none";
    void err.offsetHeight;
    err.style.animation = "";
    return;
  }
  document.getElementById("login-error").style.display = "none";

  state.name = name;
  state.position = pos;
  state.current = 0;
  state.score = 0;
  state.timerLeft = TIMER_SECONDS;
  state.startTime = new Date();

  // Перемешать банк вопросов, взять первые TOTAL_QUESTIONS
  const shuffled = shuffle(QUESTIONS_BANK).slice(0, TOTAL_QUESTIONS);
  // Для каждого вопроса перемешать варианты ответов
  state.questions = shuffled.map(q => {
    const indexed = q.options.map((opt, i) => ({ opt, correct: i === q.answer }));
    const shuffledOpts = shuffle(indexed);
    return {
      q: q.q,
      options: shuffledOpts.map(x => x.opt),
      answer: shuffledOpts.findIndex(x => x.correct)
    };
  });

  show("screen-question");
  renderQuestion();
  startTimer();
}

function startTimer() {
  clearInterval(state.timerInterval);
  updateTimerDisplay();
  state.timerInterval = setInterval(() => {
    state.timerLeft--;
    updateTimerDisplay();
    if (state.timerLeft <= 0) {
      clearInterval(state.timerInterval);
      finishTest(true);
    }
  }, 1000);
}

function updateTimerDisplay() {
  const el   = document.getElementById("timer-display");
  const wrap = document.getElementById("timer-wrap");
  el.textContent = formatTime(state.timerLeft);
  if (state.timerLeft <= 60) {
    wrap.classList.add("danger");
  } else {
    wrap.classList.remove("danger");
  }
}

function renderQuestion() {
  const total = state.questions.length;
  const idx   = state.current;
  const q     = state.questions[idx];
  const pct   = (idx / total) * 100;

  document.getElementById("q-counter").textContent  = `Вопрос ${idx + 1} из ${total}`;
  document.getElementById("q-num").textContent       = `Вопрос ${idx + 1}`;
  document.getElementById("progress-fill").style.width = `${pct}%`;

  // Анимация смены текста вопроса
  const qText = document.getElementById("q-text");
  qText.style.opacity = "0";
  qText.style.transform = "translateY(10px)";
  setTimeout(() => {
    qText.textContent = q.q;
    qText.style.transition = "opacity .35s ease, transform .35s cubic-bezier(0.25,0.46,0.45,0.94)";
    qText.style.opacity = "1";
    qText.style.transform = "translateY(0)";
  }, 80);

  const wrap = document.getElementById("options-wrap");
  wrap.innerHTML = "";
  const letters = ["А", "Б", "В", "Г"];

  q.options.forEach((opt, i) => {
    const btn = document.createElement("button");
    btn.className = "option";
    btn.innerHTML = `<span class="option-letter">${letters[i]}</span>${opt}`;
    btn.onclick = () => selectAnswer(i);
    wrap.appendChild(btn);
    // Staggered entrance — зафиксировать opacity:1 после окончания анимации
    setTimeout(() => {
      btn.classList.add("visible");
      btn.addEventListener("animationend", () => {
        btn.style.opacity = "1";
        btn.style.transform = "translateY(0)";
      }, { once: true });
    }, 120 + i * 70);
  });
}

function selectAnswer(chosen) {
  const q       = state.questions[state.current];
  const options = document.querySelectorAll(".option");
  const correct = q.answer;

  // Заблокировать все опции
  options.forEach(o => o.classList.add("disabled"));
  options[correct].classList.add("correct");
  if (chosen !== correct) {
    options[chosen].classList.add("wrong");
  } else {
    state.score++;
  }

  // Задержка перед следующим вопросом
  setTimeout(() => {
    state.current++;
    if (state.current < state.questions.length) {
      renderQuestion();
    } else {
      finishTest(false);
    }
  }, 900);
}

function finishTest(timeOut) {
  clearInterval(state.timerInterval);
  state.finishTime = new Date();

  const total = state.questions.length;
  const score = state.score;
  const pct   = Math.round((score / total) * 100);

  // Оценка
  let gradeText, gradeClass, trophy;
  if (pct >= 90)      { gradeText = "Отлично";          gradeClass = "grade-excellent";    trophy = "🏆"; }
  else if (pct >= 75) { gradeText = "Хорошо";           gradeClass = "grade-good";          trophy = "🥈"; }
  else if (pct >= 60) { gradeText = "Удовлетворительно"; gradeClass = "grade-satisfactory"; trophy = "📋"; }
  else                { gradeText = "Не пройден";        gradeClass = "grade-fail";          trophy = "📌"; }

  if (timeOut) gradeText += " (время истекло)";

  const elapsed = TIMER_SECONDS - state.timerLeft;
  const timeStr = formatTime(elapsed);

  document.getElementById("res-trophy").textContent  = trophy;
  document.getElementById("res-pct").textContent     = `${pct}%`;
  const gradeEl = document.getElementById("res-grade");
  gradeEl.textContent = gradeText;
  gradeEl.className = `result-grade ${gradeClass}`;
  document.getElementById("res-name").textContent    = state.name;
  document.getElementById("res-pos").textContent     = state.position;
  document.getElementById("res-date").textContent    = formatDate(state.finishTime);
  document.getElementById("res-correct").textContent = `${score} из ${total}`;
  document.getElementById("res-time").textContent    = timeStr;

  // Анимированный счётчик результата
  const scoreEl = document.getElementById("res-score");
  scoreEl.innerHTML = `0<span>/${total}</span>`;
  show("screen-result");
  let current = 0;
  const step = Math.ceil(score / 30);
  const counter = setInterval(() => {
    current = Math.min(current + step, score);
    scoreEl.innerHTML = `${current}<span>/${total}</span>`;
    if (current >= score) clearInterval(counter);
  }, 40);

  // Конфетти при хорошем результате
  if (pct >= 75) spawnConfetti();
}

/* ============================================================
   ДЕЙСТВИЯ НА ФИНАЛЬНОМ ЭКРАНЕ
   ============================================================ */
function restartTest() {
  clearInterval(state.timerInterval);
  document.getElementById("inp-name").value = "";
  document.getElementById("inp-pos").value = "";
  document.getElementById("login-error").style.display = "none";
  show("screen-login");
}

function downloadPDF() {
  // Формируем HTML для печати/PDF через встроенный print
  const total = state.questions.length;
  const score = state.score;
  const pct   = Math.round((score / total) * 100);
  let gradeText;
  if (pct >= 90)      gradeText = "Отлично";
  else if (pct >= 75) gradeText = "Хорошо";
  else if (pct >= 60) gradeText = "Удовлетворительно";
  else                gradeText = "Не пройден";

  const elapsed = TIMER_SECONDS - state.timerLeft;
  const timeStr = formatTime(elapsed);

  const html = `<!DOCTYPE html>
<html lang="ru"><head><meta charset="UTF-8">
<title>Результат теста — ${state.name}</title>
<style>
  body { font-family: 'Segoe UI', sans-serif; max-width: 600px; margin: 40px auto; color: #1a1a1a; }
  h1 { font-size: 22px; margin-bottom: 4px; }
  .subtitle { color: #888; font-size: 13px; margin-bottom: 24px; }
  .score { font-size: 48px; font-weight: 800; }
  .grade { display: inline-block; padding: 4px 16px; border-radius: 99px; font-weight: 700; font-size: 14px; background: #f0f0f0; margin: 8px 0 20px; }
  table { width: 100%; border-collapse: collapse; }
  td { padding: 10px 0; border-bottom: 1px solid #eee; font-size: 14px; }
  td:last-child { text-align: right; font-weight: 600; }
  footer { margin-top: 32px; font-size: 12px; color: #aaa; }
  @media print { body { margin: 20px; } }
</style></head><body>

<h1>Результат тестирования</h1>
<p class="subtitle">Премиум-стандарты обслуживания · Рестобар</p>
<div class="score">${score}/${total}</div>
<br>
<span class="grade">${gradeText} · ${pct}%</span>
<table>
  <tr><td>Сотрудник</td><td>${state.name}</td></tr>
  <tr><td>Должность</td><td>${state.position}</td></tr>
  <tr><td>Дата и время</td><td>${formatDate(state.finishTime)}</td></tr>
  <tr><td>Правильных ответов</td><td>${score} из ${total}</td></tr>
  <tr><td>Процент правильных</td><td>${pct}%</td></tr>
  <tr><td>Итоговая оценка</td><td>${gradeText}</td></tr>
  <tr><td>Затрачено времени</td><td>${timeStr}</td></tr>
</table>
<footer>Документ сформирован автоматически · ${formatDate(new Date())}</footer>
</body></html>`;

const win = window.open(””, “_blank”);
if (win) {
win.document.write(html);
win.document.close();
win.focus();
setTimeout(() => win.print(), 400);
} else {
alert(“Разрешите всплывающие окна в браузере для скачивания PDF.”);
}
}

function sendEmail() {
const total = state.questions.length;
const score = state.score;
const pct   = Math.round((score / total) * 100);
let gradeText;
if (pct >= 90)      gradeText = “Отлично”;
else if (pct >= 75) gradeText = “Хорошо”;
else if (pct >= 60) gradeText = “Удовлетворительно”;
else                gradeText = “Не пройден”;

const elapsed = TIMER_SECONDS - state.timerLeft;
const timeStr = formatTime(elapsed);

const subject = encodeURIComponent(`Результат теста: ${state.name} — ${gradeText} (${pct}%)`);
const body = encodeURIComponent(
`Результат тестирования\n` +
`Рестобар · Премиум-стандарты обслуживания\n\n` +
`Сотрудник: ${state.name}\n` +
`Должность: ${state.position}\n` +
`Дата и время: ${formatDate(state.finishTime)}\n\n` +
`Правильных ответов: ${score} из ${total}\n` +
`Процент: ${pct}%\n` +
`Итоговая оценка: ${gradeText}\n` +
`Затрачено времени: ${timeStr}\n`
);

window.location.href = `mailto:${ADMIN_EMAIL}?subject=${subject}&body=${body}`;
}
/* ============================================================
КОНФЕТТИ
============================================================ */
function spawnConfetti() {
const colors = [”#c8a96e”,”#e8c97e”,”#1a7f3c”,”#1565c0”,”#f9a8d4”,”#fcd34d”,”#6ee7b7”];
const canvas = document.createElement(“canvas”);
canvas.style.cssText = “position:fixed;top:0;left:0;width:100%;height:100%;pointer-events:none;z-index:9999;”;
document.body.appendChild(canvas);
const ctx = canvas.getContext(“2d”);
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

const pieces = Array.from({length: 90}, () => ({
x: Math.random() * canvas.width,
y: -20 - Math.random() * 120,
w: 7 + Math.random() * 8,
h: 10 + Math.random() * 10,
color: colors[Math.floor(Math.random() * colors.length)],
angle: Math.random() * Math.PI * 2,
spin: (Math.random() - .5) * .18,
vx: (Math.random() - .5) * 3.5,
vy: 2.5 + Math.random() * 3.5,
opacity: 1
}));

let frame;
let elapsed = 0;
function draw() {
ctx.clearRect(0, 0, canvas.width, canvas.height);
elapsed++;
let alive = false;
pieces.forEach(p => {
p.x += p.vx; p.y += p.vy;
p.angle += p.spin;
if (elapsed > 80) p.opacity -= .014;
if (p.opacity > 0 && p.y < canvas.height + 30) alive = true;
ctx.save();
ctx.globalAlpha = Math.max(0, p.opacity);
ctx.translate(p.x, p.y);
ctx.rotate(p.angle);
ctx.fillStyle = p.color;
ctx.fillRect(-p.w/2, -p.h/2, p.w, p.h);
ctx.restore();
});
if (alive) { frame = requestAnimationFrame(draw); }
else { cancelAnimationFrame(frame); canvas.remove(); }
}
draw();
}
</script>

</body>
</html>