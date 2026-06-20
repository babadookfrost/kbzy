# kbzy

<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="theme-color" content="#1a1a1a">
<title>КБЖУ</title>
<style>
*{box-sizing:border-box;-webkit-tap-highlight-color:transparent}
body{font-family:-apple-system,BlinkMacSystemFont,"SF Pro Text",sans-serif;margin:0;padding:0;background:#1a1a1a;color:white;padding-bottom:env(safe-area-inset-bottom)}
.safe-top{height:env(safe-area-inset-top);background:#1a1a1a}

/* Табы */
.tabs{display:flex;position:sticky;top:0;z-index:10;background:#1a1a1a;border-bottom:1px solid #333;padding:8px 15px env(safe-area-inset-top)}
.tab{flex:1;padding:10px;text-align:center;border-radius:10px;font-size:14px;font-weight:600;color:#888;cursor:pointer;transition:all .2s}
.tab.active{background:#7c9cff;color:white}

/* Контент */
.page{display:none;padding:15px;animation:fadeIn .3s}
.page.active{display:block}
@keyframes fadeIn{from{opacity:0;transform:translateY(10px)}to{opacity:1;transform:translateY(0)}}

h1{font-size:24px;margin:0 0 5px 0}
.sub{color:#888;font-size:13px;margin-bottom:20px}

/* Камера */
.camera-area{position:relative;width:100%;aspect-ratio:4/3;background:#222;border-radius:16px;overflow:hidden;margin-bottom:15px}
.camera-area img{width:100%;height:100%;object-fit:cover;display:none}
.camera-area img.show{display:block}
.camera-placeholder{position:absolute;inset:0;display:flex;flex-direction:column;align-items:center;justify-content:center;color:#888;gap:10px}
.camera-placeholder svg{width:48px;height:48px;fill:#888}
input[type=file]{display:none}

.btn{width:100%;padding:16px;border:none;border-radius:14px;font-size:16px;font-weight:600;cursor:pointer;transition:transform .1s}
.btn:active{transform:scale(0.97)}
.btn-primary{background:#7c9cff;color:white}
.btn-secondary{background:#333;color:white;margin-top:8px}
.btn:disabled{opacity:0.5;cursor:not-allowed}

/* Результат */
.result{background:#222;border-radius:16px;padding:20px;margin-top:15px;animation:fadeIn .4s}
.result h3{margin:0 0 4px 0;font-size:20px}
.result p{margin:0 0 15px 0;color:#888;font-size:13px}
.macros{display:grid;grid-template-columns:1fr 1fr;gap:10px}
.macro{background:#2a2a2a;border-radius:12px;padding:12px;text-align:center}
.macro .l{font-size:11px;color:#888;text-transform:uppercase;letter-spacing:0.5px}
.macro .v{font-size:22px;font-weight:700;margin-top:4px}
.kcal{color:#ffb86b}.pro{color:#7c9cff}.fat{color:#ffd166}.carb{color:#5ee2c6}
/* Статистика */
.stats-card{background:#222;border-radius:16px;padding:20px;margin-bottom:15px}
.stats-card h3{margin:0 0 15px 0;font-size:18px}
.stat-row{margin-bottom:14px}
.stat-header{display:flex;justify-content:space-between;margin-bottom:6px;font-size:14px}
.stat-label{color:#888}
.stat-value{font-weight:600}
.bar{height:10px;background:#333;border-radius:5px;overflow:hidden}
.bar-fill{height:100%;border-radius:5px;transition:width .5s ease}
.stats-meta{font-size:12px;color:#666;margin-top:10px}

/* История */
.hist-item{background:#222;border-radius:12px;padding:14px;margin-bottom:8px;display:flex;justify-content:space-between;align-items:center}
.hist-info h4{margin:0 0 3px 0;font-size:15px}
.hist-info span{font-size:11px;color:#888}
.hist-kcal{font-weight:700;color:#ffb86b;font-size:16px}
.empty{text-align:center;color:#666;padding:40px 0;font-size:14px}

/* Настройки */
.setting{margin-bottom:20px}
.setting label{display:block;font-size:13px;color:#888;margin-bottom:6px}
.setting input{width:100%;padding:14px;background:#222;border:1px solid #333;border-radius:12px;color:white;font-size:16px;outline:none}
.setting input:focus{border-color:#7c9cff}
.stepper{display:flex;align-items:center;gap:10px}
.stepper button{width:40px;height:40px;border-radius:10px;border:none;background:#333;color:white;font-size:20px;cursor:pointer}
.stepper span{flex:1;text-align:center;font-size:18px;font-weight:600}

.msg{padding:12px;border-radius:10px;margin:10px 0;font-size:13px;text-align:center}
.msg-ok{background:#1a3a2a;color:#5ee2c6}
.msg-err{background:#3a1a1a;color:#ff6b6b}
</style>
</head>
<body>

<div class="safe-top"></div>

<div class="tabs">
  <div class="tab active" onclick="switchTab('analyze')">📷 Анализ</div>
  <div class="tab" onclick="switchTab('stats')">📊 Статистика</div>
  <div class="tab" onclick="switchTab('history')">📜 История</div>
  <div class="tab" onclick="switchTab('settings')">⚙️</div>
</div>

<!-- АНАЛИЗ -->
<div id="analyze" class="page active">
  <h1>🍽️ КБЖУ по фото</h1>
  <div class="sub">Сфотографируй еду — Gemini посчитает калории</div>

  <div class="camera-area" onclick="document.getElementById('photo').click()">    <img id="preview">
    <div class="camera-placeholder" id="placeholder">
      <svg viewBox="0 0 24 24"><path d="M12 15.2a3.2 3.2 0 100-6.4 3.2 3.2 0 000 6.4z"/><path d="M9 2L7.17 4H4c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V6c0-1.1-.9-2-2-2h-3.17L15 2H9zm3 15c-2.76 0-5-2.24-5-5s2.24-5 5-5 5 2.24 5 5-2.24 5-5 5z"/></svg>
      <span>Нажми чтобы сделать фото</span>
    </div>
  </div>
  <input type="file" id="photo" accept="image/*" capture="environment">

  <button class="btn btn-primary" id="analyzeBtn" disabled onclick="analyzePhoto()">🧠 Анализировать</button>
  <div id="msgBox"></div>
  <div id="resultBox"></div>
</div>

<!-- СТАТИСТИКА -->
<div id="stats" class="page">
  <h1>📊 Статистика</h1>
  <div class="sub">Анализ питания за период</div>
  <div style="display:flex;gap:6px;margin-bottom:15px">
    <button class="tab active" id="stDay" onclick="renderStats('day')" style="flex:1;padding:8px;font-size:13px">День</button>
    <button class="tab" id="stWeek" onclick="renderStats('week')" style="flex:1;padding:8px;font-size:13px">Неделя</button>
    <button class="tab" id="stMonth" onclick="renderStats('month')" style="flex:1;padding:8px;font-size:13px">Месяц</button>
  </div>
  <div id="statsContent"></div>
</div>

<!-- ИСТОРИЯ -->
<div id="history" class="page">
  <h1>📜 История</h1>
  <div class="sub">Все записи хранятся на устройстве</div>
  <div id="historyList"></div>
  <button class="btn btn-secondary" onclick="clearHistory()" style="margin-top:15px;background:#3a1a1a;color:#ff6b6b">🗑️ Очистить историю</button>
</div>

<!-- НАСТРОЙКИ -->
<div id="settings" class="page">
  <h1>⚙️ Настройки</h1>
  <div class="sub">API-ключ и дневные нормы</div>

  <div class="setting">
    <label>Gemini API Key</label>
    <input type="password" id="apiKeyInput" placeholder="AIzaSy..." oninput="saveSetting('apiKey',this.value)">
  </div>

  <div class="setting">
    <label>Калории (ккал/день)</label>
    <div class="stepper">
      <button onclick="stepNorm('kcal',-50)">−</button>
      <span id="normKcal">2000</span>
      <button onclick="stepNorm('kcal',50)">+</button>
    </div>  </div>
  <div class="setting">
    <label>Белки (г/день)</label>
    <div class="stepper">
      <button onclick="stepNorm('protein',-5)">−</button>
      <span id="normProtein">100</span>
      <button onclick="stepNorm('protein',5)">+</button>
    </div>
  </div>
  <div class="setting">
    <label>Жиры (г/день)</label>
    <div class="stepper">
      <button onclick="stepNorm('fat',-5)">−</button>
      <span id="normFat">70</span>
      <button onclick="stepNorm('fat',5)">+</button>
    </div>
  </div>
  <div class="setting">
    <label>Углеводы (г/день)</label>
    <div class="stepper">
      <button onclick="stepNorm('carbs',-10)">−</button>
      <span id="normCarbs">250</span>
      <button onclick="stepNorm('carbs',10)">+</button>
    </div>
  </div>
</div>

<script>
// === ДАННЫЕ ===
var DB_KEY = 'kbju_v2';
var SETTINGS_KEY = 'kbju_settings_v2';

function getEntries() {
  try { return JSON.parse(localStorage.getItem(DB_KEY) || '[]'); }
  catch(e) { return []; }
}

function saveEntry(entry) {
  var h = getEntries();
  h.unshift(entry);
  // Храним 180 дней
  var cutoff = Date.now() - 180 * 86400000;
  h = h.filter(function(x) { return x.ts >= cutoff; });
  localStorage.setItem(DB_KEY, JSON.stringify(h));
}

function getSettings() {
  try {
    var s = JSON.parse(localStorage.getItem(SETTINGS_KEY) || '{}');
    return {      apiKey: s.apiKey || '',
      kcal: s.kcal || 2000,
      protein: s.protein || 100,
      fat: s.fat || 70,
      carbs: s.carbs || 250
    };
  } catch(e) {
    return { apiKey:'', kcal:2000, protein:100, fat:70, carbs:250 };
  }
}

function saveSetting(key, val) {
  var s = getSettings();
  s[key] = val;
  localStorage.setItem(SETTINGS_KEY, JSON.stringify(s));
}

function saveNorms() {
  var s = getSettings();
  s.kcal = parseInt(document.getElementById('normKcal').textContent);
  s.protein = parseInt(document.getElementById('normProtein').textContent);
  s.fat = parseInt(document.getElementById('normFat').textContent);
  s.carbs = parseInt(document.getElementById('normCarbs').textContent);
  localStorage.setItem(SETTINGS_KEY, JSON.stringify(s));
}

// === ТАБЫ ===
function switchTab(name) {
  var pages = document.querySelectorAll('.page');
  var tabs = document.querySelectorAll('.tabs .tab');
  for (var i = 0; i < pages.length; i++) pages[i].classList.remove('active');
  for (var i = 0; i < tabs.length; i++) tabs[i].classList.remove('active');
  document.getElementById(name).classList.add('active');
  var idx = {analyze:0, stats:1, history:2, settings:3};
  if (idx[name] !== undefined) tabs[idx[name]].classList.add('active');

  if (name === 'stats') renderStats('day');
  if (name === 'history') renderHistory();
  if (name === 'settings') loadSettings();
}

// === КАМЕРА ===
var currentImageBase64 = null;

document.getElementById('photo').onchange = function(e) {
  if (!e.target.files[0]) return;
  var reader = new FileReader();
  reader.onload = function(ev) {
    currentImageBase64 = ev.target.result;
    var img = document.getElementById('preview');    img.src = currentImageBase64;
    img.classList.add('show');
    document.getElementById('placeholder').style.display = 'none';
    document.getElementById('analyzeBtn').disabled = false;
    showMsg('Фото загружено ✓', 'ok');
  };
  reader.readAsDataURL(e.target.files[0]);
};

// === АНАЛИЗ ===
function analyzePhoto() {
  var s = getSettings();
  if (!s.apiKey) { showMsg('Сначала введи API-ключ в настройках ⚙️', 'err'); return; }
  if (!currentImageBase64) { showMsg('Выбери фото!', 'err'); return; }

  var btn = document.getElementById('analyzeBtn');
  btn.disabled = true;
  btn.textContent = '⏳ Анализирую...';
  document.getElementById('resultBox').innerHTML = '';
  showMsg('Отправляю в Gemini...', 'ok');

  var base64 = currentImageBase64.split(',')[1];
  var mime = currentImageBase64.split(';')[0].split(':')[1];
  var prompt = 'Проанализируй фото еды. Определи блюдо и оцени пищевую ценность на ВСЮ порцию на фото. Верни ТОЛЬКО JSON без пояснений: {"name":"название блюда","description":"описание и примерный вес","kcal":число,"protein":число,"fat":число,"carbs":число}. Все числа целые.';

  var body = JSON.stringify({
    contents: [{ parts: [
      { text: prompt },
      { inline_data: { mime_type: mime, data: base64 } }
    ]}]
  });

  fetch('https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=' + s.apiKey, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: body
  })
  .then(function(r) { return r.json(); })
  .then(function(data) {
    if (data.error) throw new Error(data.error.message);
    var text = data.candidates[0].content.parts[0].text;
    var clean = text.replace(/```json/g,'').replace(/```/g,'').trim();
    var start = clean.indexOf('{');
    var end = clean.lastIndexOf('}');
    if (start < 0 || end < 0) throw new Error('JSON не найден');
    clean = clean.substring(start, end + 1);
    var obj = JSON.parse(clean);

    var entry = {
      name: obj.name || 'Блюдо',      desc: obj.description || '',
      kcal: Math.round(obj.kcal || 0),
      protein: Math.round(obj.protein || 0),
      fat: Math.round(obj.fat || 0),
      carbs: Math.round(obj.carbs || 0),
      ts: Date.now()
    };
    saveEntry(entry);
    renderResult(entry);
    showMsg('Готово! ✓', 'ok');
  })
  .catch(function(err) {
    showMsg('Ошибка: ' + err.message, 'err');
  })
  .finally(function() {
    btn.disabled = false;
    btn.textContent = '🧠 Анализировать';
  });
}

function renderResult(d) {
  var html = '<div class="result"><h3>' + esc(d.name) + '</h3><p>' + esc(d.desc) + '</p><div class="macros">';
  html += '<div class="macro"><div class="l">Калории</div><div class="v kcal">' + d.kcal + '</div></div>';
  html += '<div class="macro"><div class="l">Белки</div><div class="v pro">' + d.protein + 'г</div></div>';
  html += '<div class="macro"><div class="l">Жиры</div><div class="v fat">' + d.fat + 'г</div></div>';
  html += '<div class="macro"><div class="l">Углеводы</div><div class="v carb">' + d.carbs + 'г</div></div>';
  html += '</div></div>';
  document.getElementById('resultBox').innerHTML = html;
}

function showMsg(text, type) {
  document.getElementById('msgBox').innerHTML = '<div class="msg msg-' + type + '">' + text + '</div>';
}

// === СТАТИСТИКА ===
var currentPeriod = 'day';

function renderStats(period) {
  currentPeriod = period;
  // Обновляем табы
  document.getElementById('stDay').className = period==='day' ? 'tab active' : 'tab';
  document.getElementById('stWeek').className = period==='week' ? 'tab active' : 'tab';
  document.getElementById('stMonth').className = period==='month' ? 'tab active' : 'tab';

  var entries = getEntries();
  var now = Date.now();
  var ms;
  if (period === 'day') ms = 86400000;
  else if (period === 'week') ms = 7 * 86400000;
  else ms = 30 * 86400000;
  var filtered = entries.filter(function(e) { return (now - e.ts) <= ms; });

  var totalK=0, totalP=0, totalF=0, totalC=0;
  for (var i=0; i<filtered.length; i++) {
    totalK += filtered[i].kcal;
    totalP += filtered[i].protein;
    totalF += filtered[i].fat;
    totalC += filtered[i].carbs;
  }

  var s = getSettings();
  var days = period==='day' ? 1 : period==='week' ? 7 : 30;
  var avgK = Math.round(totalK/days);
  var avgP = Math.round(totalP/days);
  var avgF = Math.round(totalF/days);
  var avgC = Math.round(totalC/days);

  var useVal = period==='day';
  var vK = useVal ? totalK : avgK;
  var vP = useVal ? totalP : avgP;
  var vF = useVal ? totalF : avgF;
  var vC = useVal ? totalC : avgC;

  var label = period==='day' ? 'Сегодня' : 'Среднее за ' + (period==='week'?'неделю':'месяц') + ' / день';

  var html = '<div class="stats-card"><h3>' + label + '</h3>';
  html += statBar('Калории', vK, s.kcal, '#ffb86b', 'ккал');
  html += statBar('Белки', vP, s.protein, '#7c9cff', 'г');
  html += statBar('Жиры', vF, s.fat, '#ffd166', 'г');
  html += statBar('Углеводы', vC, s.carbs, '#5ee2c6', 'г');
  html += '<div class="stats-meta">Записей за период: ' + filtered.length + '</div></div>';

  document.getElementById('statsContent').innerHTML = html;
}

function statBar(label, val, norm, color, unit) {
  var pct = Math.min(Math.round(val / Math.max(norm,1) * 100), 100);
  return '<div class="stat-row"><div class="stat-header"><span class="stat-label">' + label +
    '</span><span class="stat-value" style="color:' + color + '">' + val + ' ' + unit +
    ' (' + pct + '%)</span></div><div class="bar"><div class="bar-fill" style="width:' + pct +
    '%;background:' + color + '"></div></div></div>';
}

// === ИСТОРИЯ ===
function renderHistory() {
  var h = getEntries();
  if (!h.length) {
    document.getElementById('historyList').innerHTML = '<div class="empty">Нет записей.<br>Сделай первое фото еды! 📷</div>';
    return;  }
  var html = '';
  for (var i = 0; i < h.length; i++) {
    var it = h[i];
    var d = new Date(it.ts);
    var dateStr = d.toLocaleDateString('ru-RU',{day:'2-digit',month:'short'}) + ' ' + d.toLocaleTimeString('ru-RU',{hour:'2-digit',minute:'2-digit'});
    html += '<div class="hist-item"><div class="hist-info"><h4>' + esc(it.name) + '</h4><span>' + dateStr +
      ' · Б:' + it.protein + ' Ж:' + it.fat + ' У:' + it.carbs + '</span></div><div class="hist-kcal">' + it.kcal + '</div></div>';
  }
  document.getElementById('historyList').innerHTML = html;
}

function clearHistory() {
  if (confirm('Удалить всю историю?')) {
    localStorage.removeItem(DB_KEY);
    renderHistory();
  }
}

// === НАСТРОЙКИ ===
function loadSettings() {
  var s = getSettings();
  document.getElementById('apiKeyInput').value = s.apiKey;
  document.getElementById('normKcal').textContent = s.kcal;
  document.getElementById('normProtein').textContent = s.protein;
  document.getElementById('normFat').textContent = s.fat;
  document.getElementById('normCarbs').textContent = s.carbs;
}

function stepNorm(key, delta) {
  var el = document.getElementById('norm' + key.charAt(0).toUpperCase() + key.slice(1));
  var val = parseInt(el.textContent) + delta;
  if (val < 0) val = 0;
  el.textContent = val;
  saveNorms();
}

function esc(s) {
  var div = document.createElement('div');
  div.textContent = s;
  return div.innerHTML;
}

// Инициализация
loadSettings();
</script>
</body>
</html>