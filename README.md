# Anisiya034.github.io
[Uploading пойдём-завтра-в-surf-coffee.html…]()
<!doctype html>
<html lang="ru">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Пойдём завтра в Surf Coffee?</title>
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }
  body {
    font-family: 'Segoe UI', system-ui, sans-serif;
    min-height: 100vh;
    background: linear-gradient(135deg, #f6d365 0%, #fda085 50%, #f76b8a 100%);
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 20px;
    overflow: hidden;
  }
  .screen {
    display: none;
    background: #fff;
    border-radius: 24px;
    box-shadow: 0 20px 60px rgba(0,0,0,.18);
    padding: 48px 40px;
    max-width: 460px;
    width: 100%;
    text-align: center;
    animation: pop .35s ease;
  }
  .screen.active { display: block; }
  @keyframes pop { from { transform: scale(.9); opacity: 0; } to { transform: scale(1); opacity: 1; } }
  h1 { font-size: 26px; color: #2d2a32; line-height: 1.35; margin-bottom: 12px; }
  .emoji { font-size: 52px; display: block; margin-bottom: 16px; }
  .sub { color: #8a8494; font-size: 15px; margin-bottom: 30px; }
  .btn-row { display: flex; gap: 16px; justify-content: center; position: relative; min-height: 60px; }
  button {
    font-size: 18px; font-weight: 600;
    padding: 14px 40px;
    border: none; border-radius: 999px;
    cursor: pointer;
    transition: transform .15s, box-shadow .15s;
  }
  .btn-yes { background: #ff6b6b; color: #fff; box-shadow: 0 8px 20px rgba(255,107,107,.4); }
  .btn-yes:hover { transform: scale(1.06); }
  .btn-no {
    background: #fff; color: #4a4553;
    border: 2px solid #d9d4e0;
    position: absolute;
    left: 60%; top: 0;
    z-index: 5;
  }
  label { display: block; text-align: left; font-weight: 600; color: #2d2a32; margin: 20px 0 8px; font-size: 15px; }
  input {
    width: 100%;
    padding: 13px 16px;
    border: 2px solid #ebe7f0;
    border-radius: 12px;
    font-size: 16px;
    color: #2d2a32;
    outline: none;
    transition: border-color .2s;
  }
  input:focus { border-color: #ff6b6b; }
  .btn-send { background: #ff6b6b; color: #fff; width: 100%; margin-top: 28px; box-shadow: 0 8px 20px rgba(255,107,107,.4); }
  .btn-send:disabled { opacity: .6; cursor: wait; }
  .error { color: #d64545; font-size: 14px; margin-top: 12px; display: none; }
  .success .emoji { animation: bounce 1s infinite; }
  @keyframes bounce { 0%,100% { transform: translateY(0);} 50% { transform: translateY(-10px);} }
</style>
</head>
<body>

<!-- Экран 1: вопрос -->
<div class="screen active" id="screen1">
  <span class="emoji">☕️</span>
  <h1>Мы пойдем завтра в Surf Coffee?</h1>
  <p class="sub">Выбери ответ. Только подумай хорошо 😏</p>
  <div class="btn-row">
    <button class="btn-yes" id="yesBtn">Да</button>
    <button class="btn-no" id="noBtn">Нет</button>
  </div>
</div>

<!-- Экран 2: детали -->
<div class="screen" id="screen2">
  <span class="emoji">🏄‍♀️</span>
  <h1>Отличное решение!</h1>
  <p class="sub">Теперь уточним детали встречи.</p>

  <label for="timeInput">Время встречи</label>
  <input type="time" id="timeInput" step="60" />

  <label for="placeInput">Напиши, где встретимся</label>
  <input type="text" id="placeInput" placeholder="Например: у входа в Surf Coffee на Горького" />

  <button class="btn-send" id="sendBtn">Отправить ✉️</button>
  <p class="error" id="error"></p>
</div>

<!-- Экран 3: успех -->
<div class="screen success" id="screen3">
  <span class="emoji">🎉</span>
  <h1>Отправлено!</h1>
  <p class="sub" id="okText">Жду тебя завтра ☕️</p>
</div>

<script>
  var yesBtn = document.getElementById('yesBtn');
  var noBtn = document.getElementById('noBtn');
  var sendBtn = document.getElementById('sendBtn');
  var errorEl = document.getElementById('error');

  function showScreen(n) {
    document.querySelectorAll('.screen').forEach(function (s) { s.classList.remove('active'); });
    document.getElementById('screen' + n).classList.add('active');
  }

  // Кнопка "Нет" убегает
  function escapeNo() {
    var pad = 20;
    var x = Math.random() * (window.innerWidth - noBtn.offsetWidth - 2 * pad) + pad;
    var y = Math.random() * (window.innerHeight - noBtn.offsetHeight - 2 * pad) + pad;
    noBtn.style.position = 'fixed';
    noBtn.style.left = x + 'px';
    noBtn.style.top = y + 'px';
    noBtn.style.zIndex = 9999;
  }
  noBtn.addEventListener('mouseenter', escapeNo);
  noBtn.addEventListener('click', escapeNo);
  noBtn.addEventListener('touchstart', function (e) { e.preventDefault(); escapeNo(); }, { passive: false });

  yesBtn.addEventListener('click', function () {
    showScreen(2);
  });

  sendBtn.addEventListener('click', function () {
    var time = document.getElementById('timeInput').value;
    var place = document.getElementById('placeInput').value.trim();
    errorEl.style.display = 'none';

    if (!time) { errorEl.textContent = 'Пожалуйста, выбери время встречи.'; errorEl.style.display = 'block'; return; }
    if (!place) { errorEl.textContent = 'Пожалуйста, напиши, где встретимся.'; errorEl.style.display = 'block'; return; }

    sendBtn.disabled = true;
    sendBtn.textContent = 'Отправляю...';

    var text = 'Встреча в Surf Coffee!\nВремя: ' + time + '\nМесто: ' + place;

    fetch('https://formsubmit.co/ajax/ai.ilkaeva@yandex.ru', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json', 'Accept': 'application/json' },
      body: JSON.stringify({
        _subject: 'Приглашение принято: идём в Surf Coffee! ☕️',
        'Время встречи': time,
        'Место встречи': place,
        _template: 'table'
      })
    })
    .then(function (r) {
      if (!r.ok) throw new Error('bad status');
      document.getElementById('okText').textContent =
        'Время: ' + time + ' · Место: ' + place + '. Жду тебя завтра! ☕️';
      showScreen(3);
    })
    .catch(function () {
      // запасной вариант: открыть почтовый клиент с готовым письмом
      sendBtn.disabled = false;
      sendBtn.textContent = 'Отправить ✉️';
      errorEl.textContent = 'Не удалось отправить автоматически. Нажми, чтобы отправить через свою почту.';
      errorEl.style.display = 'block';
      var mailto = document.createElement('a');
      mailto.href = 'mailto:ai.ilkaeva@yandex.ru?subject=' + encodeURIComponent('Мы идём в Surf Coffee!') +
        '&body=' + encodeURIComponent(text);
      mailto.style.display = 'none';
      document.body.appendChild(mailto);
      mailto.click();
      mailto.remove();
    });
  });
</script>
</body>
</html>
