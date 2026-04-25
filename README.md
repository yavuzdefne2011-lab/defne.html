<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>CodeLab — Kodlama & Teknoloji Eğitimi</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0a0f;
    --surface: #12121a;
    --card: #1a1a26;
    --border: #2a2a3d;
    --accent: #7c6af7;
    --accent2: #f7c26a;
    --green: #4ade80;
    --text: #e8e8f0;
    --muted: #7070a0;
    --mono: 'Space Mono', monospace;
    --sans: 'DM Sans', sans-serif;
  }

  * { margin:0; padding:0; box-sizing:border-box; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: var(--sans);
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* Grid background */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(var(--border) 1px, transparent 1px),
      linear-gradient(90deg, var(--border) 1px, transparent 1px);
    background-size: 40px 40px;
    opacity: 0.3;
    pointer-events: none;
    z-index: 0;
  }

  /* ── NAV ── */
  nav {
    position: sticky; top: 0;
    z-index: 100;
    display: flex; align-items: center; justify-content: space-between;
    padding: 16px 40px;
    background: rgba(10,10,15,0.85);
    backdrop-filter: blur(12px);
    border-bottom: 1px solid var(--border);
  }
  .logo {
    font-family: var(--mono);
    font-size: 18px;
    color: var(--accent2);
    display: flex; align-items: center; gap: 8px;
  }
  .logo span { color: var(--accent); }
  nav ul { list-style:none; display:flex; gap:32px; }
  nav ul a {
    color: var(--muted); text-decoration:none;
    font-size:14px; font-weight:500;
    transition: color .2s;
  }
  nav ul a:hover { color: var(--text); }
  .nav-cta {
    background: var(--accent);
    color: #fff !important;
    padding: 8px 18px;
    border-radius: 6px;
    font-weight: 600 !important;
  }

  /* ── HERO ── */
  .hero {
    position: relative; z-index: 1;
    padding: 100px 40px 80px;
    max-width: 900px; margin: 0 auto;
    text-align: center;
  }
  .hero-badge {
    display: inline-flex; align-items: center; gap: 8px;
    background: rgba(124,106,247,0.15);
    border: 1px solid rgba(124,106,247,0.3);
    color: var(--accent);
    font-family: var(--mono); font-size: 12px;
    padding: 6px 14px; border-radius: 100px;
    margin-bottom: 28px;
    animation: fadeDown .6s ease both;
  }
  .hero-badge::before {
    content: ''; width: 6px; height: 6px;
    background: var(--accent); border-radius: 50%;
    animation: pulse 1.5s infinite;
  }
  @keyframes pulse {
    0%,100%{ opacity:1; transform:scale(1); }
    50%{ opacity:.4; transform:scale(1.4); }
  }
  h1 {
    font-family: var(--mono);
    font-size: clamp(32px, 5vw, 58px);
    line-height: 1.15;
    margin-bottom: 20px;
    animation: fadeDown .6s .1s ease both;
  }
  h1 em { color: var(--accent2); font-style:normal; }
  h1 .hl { color: var(--accent); }
  .hero p {
    color: var(--muted); font-size: 18px; line-height: 1.7;
    max-width: 580px; margin: 0 auto 40px;
    animation: fadeDown .6s .2s ease both;
  }
  .hero-btns {
    display: flex; gap: 14px; justify-content: center; flex-wrap: wrap;
    animation: fadeDown .6s .3s ease both;
  }
  .btn {
    padding: 12px 28px; border-radius: 8px;
    font-family: var(--sans); font-size: 15px; font-weight: 600;
    cursor: pointer; border: none; transition: all .2s;
    text-decoration: none; display: inline-block;
  }
  .btn-primary { background: var(--accent); color: #fff; }
  .btn-primary:hover { background: #9580ff; transform: translateY(-1px); }
  .btn-outline {
    background: transparent;
    border: 1px solid var(--border);
    color: var(--text);
  }
  .btn-outline:hover { border-color: var(--accent); color: var(--accent); }

  @keyframes fadeDown {
    from { opacity:0; transform:translateY(-16px); }
    to   { opacity:1; transform:translateY(0); }
  }

  /* ── STATS ── */
  .stats {
    position: relative; z-index: 1;
    display: flex; justify-content: center; gap: 0;
    max-width: 700px; margin: 0 auto 80px;
    border: 1px solid var(--border);
    border-radius: 12px;
    overflow: hidden;
    animation: fadeDown .6s .4s ease both;
  }
  .stat {
    flex: 1; padding: 20px 24px; text-align: center;
    border-right: 1px solid var(--border);
  }
  .stat:last-child { border-right: none; }
  .stat-num {
    font-family: var(--mono); font-size: 28px;
    color: var(--accent2); display: block;
  }
  .stat-lbl { color: var(--muted); font-size: 12px; margin-top: 4px; }

  /* ── COURSES ── */
  .section {
    position: relative; z-index: 1;
    max-width: 1100px; margin: 0 auto;
    padding: 0 40px 80px;
  }
  .section-header {
    display: flex; align-items: flex-end;
    justify-content: space-between;
    margin-bottom: 36px;
  }
  .section-tag {
    font-family: var(--mono); font-size: 11px;
    color: var(--accent); text-transform: uppercase;
    letter-spacing: 2px; margin-bottom: 8px;
  }
  .section-title {
    font-family: var(--mono); font-size: 28px;
  }

  .courses-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 20px;
  }
  .course-card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 28px;
    cursor: pointer;
    transition: all .25s;
    position: relative;
    overflow: hidden;
  }
  .course-card::before {
    content: '';
    position: absolute; top: 0; left: 0; right: 0;
    height: 2px;
    opacity: 0;
    transition: opacity .25s;
  }
  .course-card.cpp::before { background: linear-gradient(90deg, #7c6af7, #a78bfa); }
  .course-card.arduino::before { background: linear-gradient(90deg, #4ade80, #22d3ee); }
  .course-card.tinkercad::before { background: linear-gradient(90deg, #f7c26a, #fb923c); }
  .course-card:hover { border-color: var(--accent); transform: translateY(-3px); }
  .course-card:hover::before { opacity: 1; }
  .course-icon {
    width: 44px; height: 44px; border-radius: 10px;
    display: flex; align-items: center; justify-content: center;
    font-size: 22px; margin-bottom: 16px;
  }
  .cpp .course-icon { background: rgba(124,106,247,0.15); }
  .arduino .course-icon { background: rgba(74,222,128,0.15); }
  .tinkercad .course-icon { background: rgba(247,194,106,0.15); }
  .course-card h3 {
    font-family: var(--mono); font-size: 16px;
    margin-bottom: 8px;
  }
  .course-card p {
    color: var(--muted); font-size: 14px; line-height: 1.6;
    margin-bottom: 20px;
  }
  .course-meta {
    display: flex; gap: 12px; align-items: center;
  }
  .tag {
    font-family: var(--mono); font-size: 11px;
    padding: 3px 8px; border-radius: 4px;
    background: rgba(255,255,255,0.06);
    color: var(--muted);
  }
  .level-dot {
    width: 6px; height: 6px; border-radius: 50%;
    display: inline-block; margin-right: 4px;
  }
  .beginner .level-dot { background: var(--green); }
  .inter .level-dot { background: var(--accent2); }

  /* ── CHATBOT ── */
  #chatbot {
    position: relative; z-index: 1;
    max-width: 800px; margin: 0 auto 80px;
    padding: 0 40px;
  }
  .chat-container {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 16px;
    overflow: hidden;
  }
  .chat-header {
    padding: 18px 24px;
    border-bottom: 1px solid var(--border);
    display: flex; align-items: center; gap: 12px;
    background: var(--surface);
  }
  .chat-avatar {
    width: 36px; height: 36px; border-radius: 10px;
    background: linear-gradient(135deg, var(--accent), #a78bfa);
    display: flex; align-items: center; justify-content: center;
    font-size: 18px;
  }
  .chat-header-info h4 {
    font-family: var(--mono); font-size: 14px;
  }
  .chat-header-info span {
    font-size: 12px; color: var(--green);
    display: flex; align-items: center; gap: 5px;
  }
  .chat-header-info span::before {
    content: ''; width: 6px; height: 6px;
    background: var(--green); border-radius: 50%;
    animation: pulse 1.5s infinite;
  }
  .chat-messages {
    height: 380px; overflow-y: auto;
    padding: 24px; display: flex;
    flex-direction: column; gap: 16px;
  }
  .chat-messages::-webkit-scrollbar { width: 4px; }
  .chat-messages::-webkit-scrollbar-track { background: transparent; }
  .chat-messages::-webkit-scrollbar-thumb { background: var(--border); border-radius: 4px; }

  .msg {
    display: flex; gap: 10px; max-width: 85%;
    animation: msgIn .25s ease;
  }
  @keyframes msgIn {
    from { opacity:0; transform:translateY(8px); }
    to   { opacity:1; transform:translateY(0); }
  }
  .msg.user { margin-left: auto; flex-direction: row-reverse; }
  .msg-avatar {
    width: 30px; height: 30px; border-radius: 8px;
    flex-shrink: 0;
    display: flex; align-items: center; justify-content: center;
    font-size: 14px;
  }
  .msg.bot .msg-avatar { background: linear-gradient(135deg, var(--accent), #a78bfa); }
  .msg.user .msg-avatar { background: rgba(124,106,247,0.2); color: var(--accent); }
  .msg-bubble {
    padding: 12px 16px;
    border-radius: 12px;
    font-size: 14px; line-height: 1.6;
  }
  .msg.bot .msg-bubble {
    background: var(--surface);
    border: 1px solid var(--border);
    border-top-left-radius: 4px;
    color: var(--text);
  }
  .msg.user .msg-bubble {
    background: var(--accent);
    color: #fff;
    border-top-right-radius: 4px;
  }
  .msg-bubble code {
    font-family: var(--mono); font-size: 12px;
    background: rgba(0,0,0,0.3);
    padding: 2px 5px; border-radius: 4px;
  }

  .typing-dots span {
    display: inline-block; width: 6px; height: 6px;
    border-radius: 50%; background: var(--muted);
    margin: 0 2px;
    animation: bounce .8s infinite;
  }
  .typing-dots span:nth-child(2) { animation-delay: .15s; }
  .typing-dots span:nth-child(3) { animation-delay: .3s; }
  @keyframes bounce {
    0%,80%,100%{ transform:translateY(0); }
    40%{ transform:translateY(-6px); }
  }

  .chat-input-area {
    padding: 16px 20px;
    border-top: 1px solid var(--border);
    display: flex; gap: 10px; align-items: flex-end;
  }
  .chat-input {
    flex: 1;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 12px 16px;
    color: var(--text);
    font-family: var(--sans); font-size: 14px;
    resize: none; outline: none;
    transition: border-color .2s;
    min-height: 44px; max-height: 120px;
  }
  .chat-input:focus { border-color: var(--accent); }
  .chat-input::placeholder { color: var(--muted); }
  .send-btn {
    width: 44px; height: 44px; border-radius: 10px;
    background: var(--accent); border: none;
    color: #fff; cursor: pointer;
    display: flex; align-items: center; justify-content: center;
    font-size: 18px; transition: all .2s; flex-shrink: 0;
  }
  .send-btn:hover { background: #9580ff; transform: scale(1.05); }
  .send-btn:disabled { opacity: .5; cursor: not-allowed; transform: none; }

  .quick-questions {
    padding: 0 20px 14px;
    display: flex; gap: 8px; flex-wrap: wrap;
  }
  .quick-q {
    font-size: 12px; padding: 5px 12px;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 100px; cursor: pointer;
    color: var(--muted); transition: all .2s;
    font-family: var(--sans);
  }
  .quick-q:hover { border-color: var(--accent); color: var(--accent); }

  /* ── FOOTER ── */
  footer {
    position: relative; z-index: 1;
    border-top: 1px solid var(--border);
    padding: 32px 40px;
    display: flex; justify-content: space-between; align-items: center;
    color: var(--muted); font-size: 13px;
  }
  footer .footer-logo { font-family: var(--mono); color: var(--accent2); }

  /* responsive */
  @media(max-width:640px){
    nav { padding: 14px 20px; }
    nav ul { display: none; }
    .hero { padding: 60px 20px 50px; }
    .stats { margin: 0 20px 60px; }
    .section, #chatbot { padding-left: 20px; padding-right: 20px; }
    footer { flex-direction: column; gap: 10px; text-align:center; }
  }
</style>
</head>
<body>

<!-- NAV -->
<nav>
  <div class="logo">⬡ <span>Code</span>Lab</div>
  <ul>
    <li><a href="#kurslar">Kurslar</a></li>
    <li><a href="#chatbot">AI Asistan</a></li>
    <li><a href="#chatbot" class="nav-cta">Soru Sor</a></li>
  </ul>
</nav>

<!-- HERO -->
<section class="hero">
  <div class="hero-badge">🚀 Yeni sezon başladı</div>
  <h1>
    <em>Kodlamayı</em> öğren,<br>
    <span class="hl">geleceği</span> inşa et.
  </h1>
  <p>C++, Arduino ve Tinkercad ile teknoloji dünyasına adım at. 9–18 yaş arası öğrenciler için tasarlandı.</p>
  <div class="hero-btns">
    <a href="#kurslar" class="btn btn-primary">Kurslara Bak</a>
    <a href="#chatbot" class="btn btn-outline">AI'a Sor 🤖</a>
  </div>
</section>

<!-- STATS -->
<div class="stats">
  <div class="stat">
    <span class="stat-num">3</span>
    <div class="stat-lbl">Kurs</div>
  </div>
  <div class="stat">
    <span class="stat-num">9–18</span>
    <div class="stat-lbl">Yaş Aralığı</div>
  </div>
  <div class="stat">
    <span class="stat-num">100%</span>
    <div class="stat-lbl">Uygulamalı</div>
  </div>
  <div class="stat">
    <span class="stat-num">7/24</span>
    <div class="stat-lbl">AI Destek</div>
  </div>
</div>

<!-- COURSES -->
<section class="section" id="kurslar">
  <div class="section-header">
    <div>
      <div class="section-tag">// kurslar</div>
      <h2 class="section-title">Ne öğrenmek istersin?</h2>
    </div>
  </div>
  <div class="courses-grid">

    <div class="course-card cpp">
      <div class="course-icon">⚡</div>
      <h3>C++ Programlama</h3>
      <p>Değişkenler, koşullar, döngüler ve fonksiyonlarla programlamanın temellerini öğren. Gerçek kod yaz, gerçek sonuçlar gör.</p>
      <div class="course-meta">
        <span class="tag">C++</span>
        <span class="tag beginner"><span class="level-dot"></span>Başlangıç</span>
        <span class="tag">8 hafta</span>
      </div>
    </div>

    <div class="course-card arduino">
      <div class="course-icon">🔌</div>
      <h3>Arduino & Elektronik</h3>
      <p>LED'ler, sensörler ve motorlarla fiziksel dünyayı programla. Park sensöründen akıllı ev sistemine kadar her şeyi yap.</p>
      <div class="course-meta">
        <span class="tag">Arduino</span>
        <span class="tag inter"><span class="level-dot"></span>Orta</span>
        <span class="tag">10 hafta</span>
      </div>
    </div>

    <div class="course-card tinkercad">
      <div class="course-icon">🏗️</div>
      <h3>Tinkercad 3D Tasarım</h3>
      <p>3 boyutlu düşün, tasarla ve yazdır. Geleceğin şehrini tasarlamaktan kişisel aksesuarlara kadar hayal gücün sınır.</p>
      <div class="course-meta">
        <span class="tag">Tinkercad</span>
        <span class="tag beginner"><span class="level-dot"></span>Başlangıç</span>
        <span class="tag">6 hafta</span>
      </div>
    </div>

  </div>
</section>

<!-- CHATBOT -->
<section id="chatbot">
  <div style="position:relative;z-index:1; margin-bottom:24px;">
    <div class="section-tag" style="text-align:center">// ai asistan</div>
    <h2 class="section-title" style="text-align:center; font-family:var(--mono)">Aklına takılan her şeyi sor</h2>
    <p style="text-align:center; color:var(--muted); margin-top:8px; font-size:14px">C++, Arduino ve Tinkercad hakkında anında yardım al</p>
  </div>

  <div class="chat-container">
    <div class="chat-header">
      <div class="chat-avatar">🤖</div>
      <div class="chat-header-info">
        <h4>CodeLab Asistanı</h4>
        <span>Çevrimiçi</span>
      </div>
    </div>

    <div class="chat-messages" id="chatMessages">
      <div class="msg bot">
        <div class="msg-avatar">🤖</div>
        <div class="msg-bubble">
          Merhaba! Ben CodeLab'in AI asistanıyım 👋<br><br>
          C++, Arduino ve Tinkercad hakkında soru sorabilirsin. Ödev konuları, kod hataları, devre bağlantıları — her şeyde yardımcı olurum!
        </div>
      </div>
    </div>

    <div class="quick-questions">
      <button class="quick-q" onclick="sendQuick(this)">C++ değişken nedir?</button>
      <button class="quick-q" onclick="sendQuick(this)">Arduino LED nasıl yakar?</button>
      <button class="quick-q" onclick="sendQuick(this)">Tinkercad'e nasıl başlarım?</button>
      <button class="quick-q" onclick="sendQuick(this)">if-else ne işe yarar?</button>
    </div>

    <div class="chat-input-area">
      <textarea
        class="chat-input"
        id="chatInput"
        placeholder="Sorunuzu yazın..."
        rows="1"
        onkeydown="handleKey(event)"
        oninput="autoResize(this)"
      ></textarea>
      <button class="send-btn" id="sendBtn" onclick="sendMessage()">➤</button>
    </div>
  </div>
</section>

<!-- FOOTER -->
<footer>
  <div class="footer-logo">⬡ CodeLab</div>
  <div>Ortaokul ve lise öğrencileri için teknoloji eğitimi</div>
</footer>

<script>
let isLoading = false;
const messagesDiv = document.getElementById('chatMessages');
const input = document.getElementById('chatInput');
const sendBtn = document.getElementById('sendBtn');
const conversationHistory = [];

function autoResize(el) {
  el.style.height = 'auto';
  el.style.height = Math.min(el.scrollHeight, 120) + 'px';
}

function handleKey(e) {
  if (e.key === 'Enter' && !e.shiftKey) {
    e.preventDefault();
    sendMessage();
  }
}

function sendQuick(btn) {
  input.value = btn.textContent;
  sendMessage();
}

function addMsg(role, text) {
  const isBot = role === 'bot';
  const div = document.createElement('div');
  div.className = `msg ${role}`;
  div.innerHTML = `
    <div class="msg-avatar">${isBot ? '🤖' : '👤'}</div>
    <div class="msg-bubble">${formatText(text)}</div>
  `;
  messagesDiv.appendChild(div);
  messagesDiv.scrollTop = messagesDiv.scrollHeight;
  return div;
}

function formatText(text) {
  return text
    .replace(/`([^`]+)`/g, '<code>$1</code>')
    .replace(/\*\*(.+?)\*\*/g, '<strong>$1</strong>')
    .replace(/\n/g, '<br>');
}

function addTyping() {
  const div = document.createElement('div');
  div.className = 'msg bot';
  div.id = 'typing';
  div.innerHTML = `
    <div class="msg-avatar">🤖</div>
    <div class="msg-bubble"><div class="typing-dots"><span></span><span></span><span></span></div></div>
  `;
  messagesDiv.appendChild(div);
  messagesDiv.scrollTop = messagesDiv.scrollHeight;
}

async function sendMessage() {
  const text = input.value.trim();
  if (!text || isLoading) return;

  isLoading = true;
  sendBtn.disabled = true;
  input.value = '';
  input.style.height = 'auto';

  addMsg('user', text);
  conversationHistory.push({ role: 'user', content: text });
  addTyping();

  try {
    const response = await fetch('/api/chat', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ messages: conversationHistory })
    });

    const data = await response.json();
    document.getElementById('typing')?.remove();

    const reply = data.content?.[0]?.text || 'Bir hata oluştu, tekrar dene.';
    addMsg('bot', reply);
    conversationHistory.push({ role: 'assistant', content: reply });

  } catch (err) {
    document.getElementById('typing')?.remove();
    addMsg('bot', 'Bağlantı hatası oluştu. 🔄');
  }

  isLoading = false;
  sendBtn.disabled = false;
  input.focus();
}
</script>
</body>
</html>
