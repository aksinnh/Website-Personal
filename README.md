<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>TKJ Interactive Learning - Jaringan Komputer</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Share+Tech+Mono&family=Exo+2:wght@300;400;600&display=swap" rel="stylesheet">
<style>
  :root {
    --neon-cyan: #00f5ff;
    --neon-green: #39ff14;
    --neon-orange: #ff6b35;
    --neon-purple: #bf5fff;
    --dark-bg: #020b18;
    --panel-bg: rgba(0, 20, 40, 0.85);
    --border-glow: rgba(0, 245, 255, 0.4);
    --text-dim: #7ab3cc;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    font-family: 'Exo 2', sans-serif;
    background: var(--dark-bg);
    color: #e0f4ff;
    min-height: 100vh;
    overflow-x: hidden;
    position: relative;
  }

  /* Animated grid background */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(0,245,255,0.04) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0,245,255,0.04) 1px, transparent 1px);
    background-size: 40px 40px;
    animation: gridMove 20s linear infinite;
    pointer-events: none;
    z-index: 0;
  }

  @keyframes gridMove {
    0% { transform: translateY(0); }
    100% { transform: translateY(40px); }
  }

  /* Floating particles */
  .particle {
    position: fixed;
    width: 2px;
    height: 2px;
    background: var(--neon-cyan);
    border-radius: 50%;
    animation: float linear infinite;
    opacity: 0;
    pointer-events: none;
    z-index: 0;
  }

  @keyframes float {
    0% { transform: translateY(100vh) translateX(0); opacity: 0; }
    10% { opacity: 1; }
    90% { opacity: 1; }
    100% { transform: translateY(-10vh) translateX(50px); opacity: 0; }
  }

  .wrapper {
    position: relative;
    z-index: 1;
    max-width: 860px;
    margin: 0 auto;
    padding: 20px 16px;
  }

  /* Header */
  header {
    text-align: center;
    margin-bottom: 28px;
  }

  .header-badge {
    display: inline-block;
    background: linear-gradient(135deg, rgba(0,245,255,0.15), rgba(191,95,255,0.15));
    border: 1px solid var(--border-glow);
    border-radius: 4px;
    padding: 4px 14px;
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    color: var(--neon-cyan);
    letter-spacing: 3px;
    text-transform: uppercase;
    margin-bottom: 10px;
  }

  h1 {
    font-family: 'Orbitron', monospace;
    font-size: clamp(18px, 4vw, 32px);
    font-weight: 900;
    background: linear-gradient(90deg, var(--neon-cyan), var(--neon-purple), var(--neon-cyan));
    background-size: 200% auto;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    animation: shimmer 3s linear infinite;
    letter-spacing: 2px;
    line-height: 1.2;
  }

  @keyframes shimmer {
    0% { background-position: 0% center; }
    100% { background-position: 200% center; }
  }

  .subtitle {
    font-size: 13px;
    color: var(--text-dim);
    margin-top: 6px;
    font-family: 'Share Tech Mono', monospace;
    letter-spacing: 1px;
  }

  /* Score bar */
  .scorebar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background: var(--panel-bg);
    border: 1px solid var(--border-glow);
    border-radius: 8px;
    padding: 10px 18px;
    margin-bottom: 22px;
    backdrop-filter: blur(8px);
  }

  .score-item {
    text-align: center;
  }

  .score-label {
    font-family: 'Share Tech Mono', monospace;
    font-size: 9px;
    color: var(--text-dim);
    letter-spacing: 2px;
    text-transform: uppercase;
  }

  .score-value {
    font-family: 'Orbitron', monospace;
    font-size: 20px;
    font-weight: 700;
    color: var(--neon-cyan);
    text-shadow: 0 0 15px var(--neon-cyan);
    transition: all 0.3s;
  }

  .score-value.pulse {
    animation: scorePulse 0.4s ease;
  }

  @keyframes scorePulse {
    0% { transform: scale(1); }
    50% { transform: scale(1.4); color: var(--neon-green); }
    100% { transform: scale(1); }
  }

  /* Progress bar */
  .progress-wrap {
    margin-bottom: 22px;
  }

  .progress-label {
    display: flex;
    justify-content: space-between;
    font-family: 'Share Tech Mono', monospace;
    font-size: 10px;
    color: var(--text-dim);
    margin-bottom: 6px;
    letter-spacing: 1px;
  }

  .progress-bar {
    height: 6px;
    background: rgba(0,245,255,0.1);
    border-radius: 10px;
    border: 1px solid rgba(0,245,255,0.2);
    overflow: hidden;
  }

  .progress-fill {
    height: 100%;
    background: linear-gradient(90deg, var(--neon-cyan), var(--neon-purple));
    border-radius: 10px;
    transition: width 0.6s cubic-bezier(0.25, 1, 0.5, 1);
    box-shadow: 0 0 10px var(--neon-cyan);
  }

  /* Game screens */
  .screen { display: none; }
  .screen.active { display: block; }

  /* Menu screen */
  .menu-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    gap: 14px;
    margin-top: 20px;
  }

  .topic-card {
    background: var(--panel-bg);
    border: 1px solid var(--border-glow);
    border-radius: 10px;
    padding: 20px 16px;
    cursor: pointer;
    transition: all 0.3s;
    text-align: center;
    backdrop-filter: blur(8px);
    position: relative;
    overflow: hidden;
  }

  .topic-card::before {
    content: '';
    position: absolute;
    top: -50%;
    left: -50%;
    width: 200%;
    height: 200%;
    background: radial-gradient(circle, rgba(0,245,255,0.06) 0%, transparent 60%);
    transform: scale(0);
    transition: transform 0.5s;
  }

  .topic-card:hover::before { transform: scale(1); }

  .topic-card:hover {
    border-color: var(--neon-cyan);
    box-shadow: 0 0 20px rgba(0,245,255,0.2), inset 0 0 20px rgba(0,245,255,0.05);
    transform: translateY(-3px);
  }

  .topic-icon {
    font-size: 38px;
    margin-bottom: 10px;
    display: block;
    filter: drop-shadow(0 0 8px rgba(0,245,255,0.5));
  }

  .topic-name {
    font-family: 'Orbitron', monospace;
    font-size: 11px;
    font-weight: 700;
    color: var(--neon-cyan);
    letter-spacing: 1px;
    text-transform: uppercase;
    margin-bottom: 5px;
  }

  .topic-desc {
    font-size: 11px;
    color: var(--text-dim);
    line-height: 1.5;
  }

  .topic-count {
    display: inline-block;
    margin-top: 8px;
    padding: 2px 10px;
    background: rgba(0,245,255,0.1);
    border: 1px solid rgba(0,245,255,0.3);
    border-radius: 20px;
    font-family: 'Share Tech Mono', monospace;
    font-size: 10px;
    color: var(--neon-cyan);
  }

  /* Question screen */
  .question-header {
    display: flex;
    align-items: center;
    gap: 12px;
    margin-bottom: 18px;
  }

  .topic-tag {
    font-family: 'Share Tech Mono', monospace;
    font-size: 10px;
    padding: 4px 12px;
    background: rgba(191,95,255,0.15);
    border: 1px solid rgba(191,95,255,0.4);
    border-radius: 4px;
    color: var(--neon-purple);
    letter-spacing: 2px;
    text-transform: uppercase;
  }

  .q-number {
    font-family: 'Orbitron', monospace;
    font-size: 12px;
    color: var(--text-dim);
  }

  .question-box {
    background: var(--panel-bg);
    border: 1px solid var(--border-glow);
    border-radius: 12px;
    padding: 24px;
    margin-bottom: 18px;
    backdrop-filter: blur(8px);
    position: relative;
  }

  .question-box::before {
    content: '?';
    position: absolute;
    right: 20px;
    top: 50%;
    transform: translateY(-50%);
    font-family: 'Orbitron', monospace;
    font-size: 80px;
    font-weight: 900;
    color: rgba(0,245,255,0.04);
    line-height: 1;
    pointer-events: none;
  }

  .question-text {
    font-size: clamp(14px, 2.5vw, 17px);
    font-weight: 400;
    line-height: 1.7;
    color: #c8e8ff;
  }

  /* Visual diagram area */
  .question-visual {
    background: rgba(0,0,0,0.4);
    border: 1px dashed rgba(0,245,255,0.2);
    border-radius: 8px;
    padding: 14px;
    margin-top: 14px;
    font-family: 'Share Tech Mono', monospace;
    font-size: 12px;
    color: var(--neon-green);
    line-height: 1.8;
    text-align: center;
    white-space: pre-wrap;
  }

  .options-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 10px;
    margin-bottom: 16px;
  }

  @media (max-width: 500px) {
    .options-grid { grid-template-columns: 1fr; }
  }

  .option-btn {
    background: rgba(0, 20, 40, 0.7);
    border: 1px solid rgba(0,245,255,0.25);
    border-radius: 8px;
    padding: 14px 16px;
    cursor: pointer;
    color: #c8e8ff;
    font-family: 'Exo 2', sans-serif;
    font-size: 13px;
    text-align: left;
    transition: all 0.2s;
    display: flex;
    align-items: center;
    gap: 10px;
    line-height: 1.4;
  }

  .option-letter {
    font-family: 'Orbitron', monospace;
    font-size: 11px;
    font-weight: 700;
    color: var(--neon-cyan);
    background: rgba(0,245,255,0.1);
    border: 1px solid rgba(0,245,255,0.3);
    border-radius: 4px;
    width: 26px;
    height: 26px;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
  }

  .option-btn:hover:not(:disabled) {
    border-color: var(--neon-cyan);
    background: rgba(0,245,255,0.08);
    transform: translateX(3px);
    box-shadow: 0 0 12px rgba(0,245,255,0.15);
  }

  .option-btn.correct {
    border-color: var(--neon-green) !important;
    background: rgba(57,255,20,0.12) !important;
    box-shadow: 0 0 20px rgba(57,255,20,0.3);
  }

  .option-btn.correct .option-letter {
    background: rgba(57,255,20,0.2);
    border-color: var(--neon-green);
    color: var(--neon-green);
  }

  .option-btn.wrong {
    border-color: var(--neon-orange) !important;
    background: rgba(255,107,53,0.1) !important;
    box-shadow: 0 0 15px rgba(255,107,53,0.2);
  }

  .option-btn.wrong .option-letter {
    background: rgba(255,107,53,0.2);
    border-color: var(--neon-orange);
    color: var(--neon-orange);
  }

  .option-btn:disabled { cursor: default; }

  /* Feedback */
  .feedback-box {
    border-radius: 8px;
    padding: 14px 18px;
    margin-bottom: 14px;
    display: none;
    animation: fadeSlide 0.3s ease;
    font-size: 13px;
    line-height: 1.6;
  }

  @keyframes fadeSlide {
    from { opacity: 0; transform: translateY(-8px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .feedback-box.correct {
    background: rgba(57,255,20,0.08);
    border: 1px solid rgba(57,255,20,0.4);
    color: #aaffaa;
    display: block;
  }

  .feedback-box.wrong {
    background: rgba(255,107,53,0.08);
    border: 1px solid rgba(255,107,53,0.4);
    color: #ffbbaa;
    display: block;
  }

  .feedback-title {
    font-family: 'Orbitron', monospace;
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 2px;
    margin-bottom: 4px;
  }

  /* Buttons */
  .btn {
    font-family: 'Orbitron', monospace;
    font-size: 12px;
    font-weight: 700;
    letter-spacing: 2px;
    padding: 12px 28px;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    transition: all 0.3s;
    text-transform: uppercase;
  }

  .btn-primary {
    background: linear-gradient(135deg, rgba(0,245,255,0.2), rgba(191,95,255,0.2));
    border: 1px solid var(--neon-cyan);
    color: var(--neon-cyan);
    box-shadow: 0 0 15px rgba(0,245,255,0.2);
  }

  .btn-primary:hover {
    background: linear-gradient(135deg, rgba(0,245,255,0.3), rgba(191,95,255,0.3));
    box-shadow: 0 0 25px rgba(0,245,255,0.4);
    transform: translateY(-2px);
  }

  .btn-secondary {
    background: transparent;
    border: 1px solid rgba(0,245,255,0.3);
    color: var(--text-dim);
  }

  .btn-secondary:hover {
    border-color: var(--neon-cyan);
    color: var(--neon-cyan);
  }

  .actions { display: flex; gap: 10px; flex-wrap: wrap; }

  /* Result screen */
  .result-container {
    text-align: center;
    padding: 30px 20px;
  }

  .result-ring {
    width: 140px;
    height: 140px;
    border-radius: 50%;
    border: 3px solid rgba(0,245,255,0.2);
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 0 auto 24px;
    position: relative;
    animation: ringGlow 2s ease-in-out infinite alternate;
  }

  @keyframes ringGlow {
    from { box-shadow: 0 0 20px rgba(0,245,255,0.2); border-color: rgba(0,245,255,0.3); }
    to { box-shadow: 0 0 40px rgba(0,245,255,0.5), 0 0 80px rgba(191,95,255,0.2); border-color: var(--neon-cyan); }
  }

  .result-percent {
    font-family: 'Orbitron', monospace;
    font-size: 38px;
    font-weight: 900;
    color: var(--neon-cyan);
    text-shadow: 0 0 20px var(--neon-cyan);
    line-height: 1;
  }

  .result-percent span {
    font-size: 16px;
    display: block;
    color: var(--text-dim);
    margin-top: 4px;
  }

  .result-grade {
    font-family: 'Orbitron', monospace;
    font-size: 22px;
    font-weight: 900;
    margin-bottom: 8px;
  }

  .result-grade.S { color: var(--neon-cyan); text-shadow: 0 0 20px var(--neon-cyan); }
  .result-grade.A { color: var(--neon-green); text-shadow: 0 0 15px var(--neon-green); }
  .result-grade.B { color: #ffd700; text-shadow: 0 0 15px #ffd700; }
  .result-grade.C { color: var(--neon-orange); }
  .result-grade.D { color: #ff4444; }

  .result-message {
    color: var(--text-dim);
    font-size: 13px;
    margin-bottom: 24px;
  }

  .result-stats {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;
    margin-bottom: 24px;
  }

  .stat-box {
    background: var(--panel-bg);
    border: 1px solid var(--border-glow);
    border-radius: 8px;
    padding: 12px;
  }

  .stat-num {
    font-family: 'Orbitron', monospace;
    font-size: 20px;
    font-weight: 700;
    color: var(--neon-cyan);
  }

  .stat-lbl {
    font-size: 10px;
    color: var(--text-dim);
    font-family: 'Share Tech Mono', monospace;
    letter-spacing: 1px;
    text-transform: uppercase;
  }

  /* Typewriter */
  .typewriter {
    overflow: hidden;
    border-right: 2px solid var(--neon-cyan);
    white-space: nowrap;
    animation: typing 2s steps(30), blink 0.7s step-end infinite;
    font-family: 'Share Tech Mono', monospace;
    font-size: 13px;
    color: var(--neon-cyan);
    margin-bottom: 12px;
    max-width: fit-content;
    margin-left: auto;
    margin-right: auto;
  }

  @keyframes typing {
    from { width: 0; }
    to { width: 100%; }
  }

  @keyframes blink {
    50% { border-color: transparent; }
  }

  /* Streaks & combo */
  .combo-badge {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-family: 'Orbitron', monospace;
    font-size: 32px;
    font-weight: 900;
    color: var(--neon-green);
    text-shadow: 0 0 30px var(--neon-green);
    pointer-events: none;
    z-index: 100;
    animation: comboPop 1s ease forwards;
    opacity: 0;
  }

  @keyframes comboPop {
    0% { opacity: 0; transform: translate(-50%, -50%) scale(0.5); }
    20% { opacity: 1; transform: translate(-50%, -50%) scale(1.3); }
    70% { opacity: 1; transform: translate(-50%, -60%) scale(1); }
    100% { opacity: 0; transform: translate(-50%, -80%) scale(0.8); }
  }

  /* Network animation */
  .network-visual {
    width: 100%;
    height: 100px;
    position: relative;
    margin: 10px 0;
    overflow: hidden;
  }

  .net-node {
    position: absolute;
    width: 10px;
    height: 10px;
    background: var(--neon-cyan);
    border-radius: 50%;
    box-shadow: 0 0 8px var(--neon-cyan);
  }

  .net-pulse {
    position: absolute;
    width: 6px;
    height: 6px;
    background: var(--neon-green);
    border-radius: 50%;
    animation: pulseMoveA 3s linear infinite;
    box-shadow: 0 0 8px var(--neon-green);
  }

  @keyframes pulseMoveA {
    0% { left: 10%; top: 50%; opacity: 0; }
    10% { opacity: 1; }
    90% { opacity: 1; }
    100% { left: 90%; top: 50%; opacity: 0; }
  }
</style>
</head>
<body>

<!-- Particles -->
<div class="particle" style="left:10%;animation-duration:8s;animation-delay:0s"></div>
<div class="particle" style="left:25%;animation-duration:12s;animation-delay:2s"></div>
<div class="particle" style="left:45%;animation-duration:9s;animation-delay:4s"></div>
<div class="particle" style="left:65%;animation-duration:11s;animation-delay:1s"></div>
<div class="particle" style="left:80%;animation-duration:7s;animation-delay:3s"></div>

<div class="wrapper">

  <!-- Header -->
  <header>
    <div class="header-badge">SMK · TKJ · INTERAKTIF</div>
    <h1>TEKNIK KOMPUTER<br>&amp; JARINGAN</h1>
    <p class="subtitle">&gt;_ SISTEM PEMBELAJARAN INTERAKTIF v2.0</p>
  </header>

  <!-- Score Bar -->
  <div class="scorebar">
    <div class="score-item">
      <div class="score-label">SKOR</div>
      <div class="score-value" id="scoreDisplay">0</div>
    </div>
    <div class="score-item">
      <div class="score-label">STREAK</div>
      <div class="score-value" id="streakDisplay" style="color:var(--neon-green)">0🔥</div>
    </div>
    <div class="score-item">
      <div class="score-label">SOAL</div>
      <div class="score-value" id="questionDisplay" style="color:var(--neon-purple)">-</div>
    </div>
    <div class="score-item">
      <div class="score-label">BENAR</div>
      <div class="score-value" id="correctDisplay" style="color:var(--neon-orange)">0</div>
    </div>
  </div>

  <!-- Progress -->
  <div class="progress-wrap">
    <div class="progress-label">
      <span id="progressLabel">Pilih topik untuk memulai</span>
      <span id="progressPct">0%</span>
    </div>
    <div class="progress-bar"><div class="progress-fill" id="progressFill" style="width:0%"></div></div>
  </div>

  <!-- ===== SCREEN: MENU ===== -->
  <div class="screen active" id="screenMenu">
    <div class="typewriter">&gt;_ Selamat datang! Pilih topik pembelajaran:</div>
    <div class="menu-grid" id="menuGrid"></div>
  </div>

  <!-- ===== SCREEN: QUESTION ===== -->
  <div class="screen" id="screenQuestion">
    <div class="question-header">
      <div class="topic-tag" id="qTopicTag">JARINGAN</div>
      <div class="q-number" id="qNumber">Soal 1 / 5</div>
    </div>

    <div class="question-box">
      <p class="question-text" id="questionText">Loading...</p>
      <div class="question-visual" id="questionVisual" style="display:none"></div>
    </div>

    <div class="options-grid" id="optionsGrid"></div>

    <div class="feedback-box" id="feedbackBox">
      <div class="feedback-title" id="feedbackTitle"></div>
      <div id="feedbackText"></div>
    </div>

    <div class="actions">
      <button class="btn btn-primary" id="btnNext" style="display:none" onclick="nextQuestion()">SOAL BERIKUTNYA ›</button>
      <button class="btn btn-secondary" onclick="goMenu()">⌂ MENU</button>
    </div>
  </div>

  <!-- ===== SCREEN: RESULT ===== -->
  <div class="screen" id="screenResult">
    <div class="result-container">
      <div class="result-ring">
        <div class="result-percent" id="resultPct">0%<span id="resultFraction"></span></div>
      </div>
      <div class="result-grade" id="resultGrade">A</div>
      <div class="result-message" id="resultMessage"></div>
      <div class="result-stats">
        <div class="stat-box">
          <div class="stat-num" id="statCorrect">0</div>
          <div class="stat-lbl">Benar</div>
        </div>
        <div class="stat-box">
          <div class="stat-num" id="statWrong">0</div>
          <div class="stat-lbl">Salah</div>
        </div>
        <div class="stat-box">
          <div class="stat-num" id="statScore">0</div>
          <div class="stat-lbl">Skor</div>
        </div>
      </div>
      <div class="actions" style="justify-content:center">
        <button class="btn btn-primary" onclick="retryTopic()">↺ ULANGI</button>
        <button class="btn btn-secondary" onclick="goMenu()">⌂ MENU</button>
      </div>
    </div>
  </div>

</div>

<script>
// ============================================================
// DATA SOAL TKJ
// ============================================================
const TOPICS = [
  {
    id: 'osi',
    name: 'Model OSI',
    icon: '🔗',
    desc: 'Layer OSI & fungsinya',
    color: '#00f5ff',
    questions: [
      {
        q: "Berapa jumlah layer pada model OSI (Open System Interconnection)?",
        visual: null,
        options: ["5 layer","7 layer","4 layer","9 layer"],
        answer: 1,
        explanation: "Model OSI terdiri dari 7 layer: Physical, Data Link, Network, Transport, Session, Presentation, dan Application."
      },
      {
        q: "Layer manakah pada model OSI yang bertanggung jawab untuk pengalamatan IP dan routing?",
        visual: "[ Application ]  Layer 7\n[ Presentation ] Layer 6\n[ Session     ]  Layer 5\n[ Transport   ]  Layer 4\n[   ???       ]  Layer 3  ← IP & Routing\n[ Data Link   ]  Layer 2\n[ Physical    ]  Layer 1",
        options: ["Transport Layer","Data Link Layer","Network Layer","Session Layer"],
        answer: 2,
        explanation: "Network Layer (Layer 3) bertanggung jawab atas pengalamatan logis (IP) dan routing paket data antar jaringan."
      },
      {
        q: "Protocol TCP dan UDP bekerja pada layer berapa dalam model OSI?",
        options: ["Layer 3 - Network","Layer 4 - Transport","Layer 5 - Session","Layer 2 - Data Link"],
        answer: 1,
        explanation: "TCP (Transmission Control Protocol) dan UDP (User Datagram Protocol) bekerja pada Layer 4 yaitu Transport Layer."
      },
      {
        q: "Layer yang mengatur format data agar dapat dimengerti oleh aplikasi adalah...",
        options: ["Application Layer","Session Layer","Presentation Layer","Transport Layer"],
        answer: 2,
        explanation: "Presentation Layer (Layer 6) bertugas mengatur format/representasi data, enkripsi, dan kompresi agar bisa dimengerti oleh Application Layer."
      },
      {
        q: "MAC Address digunakan pada layer OSI yang mana?",
        options: ["Physical Layer","Network Layer","Data Link Layer","Transport Layer"],
        answer: 2,
        explanation: "MAC Address (Media Access Control) bekerja pada Data Link Layer (Layer 2) sebagai pengalamatan fisik perangkat jaringan."
      }
    ]
  },
  {
    id: 'ip',
    name: 'Pengalamatan IP',
    icon: '🌐',
    desc: 'IP Address & Subnetting',
    color: '#39ff14',
    questions: [
      {
        q: "Berapa bit panjang alamat IPv4?",
        options: ["16 bit","32 bit","64 bit","128 bit"],
        answer: 1,
        explanation: "IPv4 menggunakan alamat 32 bit yang biasanya ditulis dalam notasi desimal bertitik (dotted decimal), contoh: 192.168.1.1"
      },
      {
        q: "IP Address 192.168.1.100/24 termasuk kelas berapa dan berapa jumlah host valid yang tersedia?",
        visual: "  Network  | Host\n192.168.1  |  100\n/24 = 255.255.255.0\nTotal Host = 2^8 = 256\nNetwork (0) + Broadcast (255) = 2 reserved",
        options: ["Kelas A, 254 host","Kelas B, 256 host","Kelas C, 254 host","Kelas C, 256 host"],
        answer: 2,
        explanation: "192.168.x.x adalah kelas C dengan subnet /24 (255.255.255.0). Jumlah host valid = 2^8 - 2 = 254 (dikurangi network address & broadcast)."
      },
      {
        q: "Subnet mask 255.255.255.0 dalam notasi CIDR adalah...",
        options: ["/22","/23","/24","/25"],
        answer: 2,
        explanation: "255.255.255.0 = 11111111.11111111.11111111.00000000 memiliki 24 bit bernilai 1, sehingga notasi CIDR-nya adalah /24."
      },
      {
        q: "Manakah yang termasuk IP Address Private (RFC 1918)?",
        options: ["172.16.0.0","8.8.8.8","1.1.1.1","200.200.200.1"],
        answer: 0,
        explanation: "Range IP Private: 10.0.0.0/8, 172.16.0.0/12, dan 192.168.0.0/16. Sedangkan 8.8.8.8 (Google DNS) dan 1.1.1.1 (Cloudflare) adalah IP Public."
      },
      {
        q: "Berapa jumlah bit pada alamat IPv6?",
        visual: "IPv6 Example:\n2001:0db8:85a3:0000:0000:8a2e:0370:7334\n[  8 kelompok × 16 bit = ??? bit  ]",
        options: ["32 bit","64 bit","128 bit","256 bit"],
        answer: 2,
        explanation: "IPv6 menggunakan alamat 128 bit (8 kelompok x 16 bit), jauh lebih besar dari IPv4 (32 bit) untuk mengatasi keterbatasan alamat."
      }
    ]
  },
  {
    id: 'hardware',
    name: 'Perangkat Jaringan',
    icon: '🖧',
    desc: 'Router, Switch, & Hub',
    color: '#bf5fff',
    questions: [
      {
        q: "Perangkat jaringan yang bekerja pada Layer 3 (Network Layer) dan bertugas menentukan jalur terbaik untuk paket data adalah...",
        options: ["Hub","Switch","Router","Bridge"],
        answer: 2,
        explanation: "Router bekerja pada Layer 3 (Network Layer), bertugas menentukan rute terbaik untuk paket data berdasarkan IP Address dan tabel routing."
      },
      {
        q: "Apa perbedaan utama antara Hub dan Switch dalam jaringan?",
        visual: "HUB:    PC1 → HUB → broadcast ke semua\nSWITCH: PC1 → SW  → hanya ke PC tujuan\n\nEfisiensi: HUB < SWITCH",
        options: ["Hub lebih cepat dari Switch","Switch meneruskan ke semua port, Hub hanya ke port tujuan","Hub bekerja di Layer 2, Switch di Layer 1","Switch meneruskan ke port tujuan saja, Hub ke semua port"],
        answer: 3,
        explanation: "Hub meneruskan data ke SEMUA port (broadcast) tanpa mengenal MAC Address. Switch lebih cerdas karena meneruskan data hanya ke port tujuan berdasarkan MAC Address table."
      },
      {
        q: "Access Point (AP) pada jaringan WiFi berfungsi sebagai...",
        options: ["Menghubungkan jaringan berbeda","Memperkuat sinyal kabel","Jembatan antara jaringan kabel dan nirkabel","Memberi alamat IP otomatis"],
        answer: 2,
        explanation: "Access Point berfungsi sebagai jembatan (bridge) antara jaringan kabel (wired/LAN) dan jaringan nirkabel (wireless), memungkinkan perangkat WiFi terhubung ke jaringan."
      },
      {
        q: "Kabel UTP (Unshielded Twisted Pair) kategori berapa yang mendukung kecepatan hingga 1 Gbps?",
        options: ["Cat 3","Cat 5","Cat 5e / Cat 6","Cat 3e"],
        answer: 2,
        explanation: "Kabel Cat 5e mendukung hingga 1 Gbps pada jarak 100 meter, sementara Cat 6 mendukung 10 Gbps pada jarak lebih pendek. Cat 3 hanya 10 Mbps."
      },
      {
        q: "Susunan kabel UTP untuk konfigurasi STRAIGHT-THROUGH pada ujung pertama menggunakan standar...",
        visual: "Pin: 1  2  3  4  5  6  7  8\nT568A: Putih-Hijau, Hijau, Putih-Oranye, Biru...\nT568B: Putih-Oranye, Oranye, Putih-Hijau, Biru...",
        options: ["T568A di kedua ujung","T568B di kedua ujung, atau T568A di kedua ujung","T568A di satu ujung, T568B di ujung lain","Bebas, tidak ada standar"],
        answer: 1,
        explanation: "Kabel Straight-Through menggunakan standar yang SAMA di kedua ujung (T568A-T568A atau T568B-T568B). Crossover menggunakan T568A di satu ujung dan T568B di ujung lain."
      }
    ]
  },
  {
    id: 'protocol',
    name: 'Protokol Jaringan',
    icon: '📡',
    desc: 'TCP/IP, DNS, DHCP, HTTP',
    color: '#ff6b35',
    questions: [
      {
        q: "DHCP (Dynamic Host Configuration Protocol) berfungsi untuk...",
        options: ["Menerjemahkan domain ke IP","Memberikan IP Address otomatis ke client","Mengamankan koneksi jaringan","Mengirim email"],
        answer: 1,
        explanation: "DHCP secara otomatis memberikan konfigurasi jaringan kepada client, termasuk IP Address, Subnet Mask, Default Gateway, dan DNS Server."
      },
      {
        q: "Port berapa yang digunakan oleh protokol HTTPS (HTTP Secure)?",
        visual: "HTTP  → Port 80\nHTTPS → Port ???\nFTP   → Port 21\nSSH   → Port 22\nSMTP  → Port 25",
        options: ["Port 80","Port 8080","Port 443","Port 8443"],
        answer: 2,
        explanation: "HTTPS (HTTP Secure) menggunakan Port 443. HTTPS mengenkripsi komunikasi menggunakan TLS/SSL untuk keamanan data yang ditransmisikan."
      },
      {
        q: "DNS (Domain Name System) berfungsi untuk...",
        options: ["Memberi IP Address ke perangkat","Menerjemahkan nama domain ke IP Address","Mengamankan jaringan","Menghubungkan jaringan berbeda"],
        answer: 1,
        explanation: "DNS menerjemahkan nama domain yang mudah diingat manusia (contoh: www.google.com) menjadi IP Address (contoh: 142.250.4.100) yang digunakan komputer."
      },
      {
        q: "Protokol mana yang CONNECTION-ORIENTED (memastikan data terkirim dengan handshake)?",
        options: ["UDP","ICMP","TCP","ARP"],
        answer: 2,
        explanation: "TCP (Transmission Control Protocol) adalah connection-oriented dengan proses 3-way handshake (SYN→SYN-ACK→ACK). UDP adalah connectionless yang lebih cepat tapi tidak menjamin pengiriman."
      },
      {
        q: "Perintah 'ping' menggunakan protokol apa?",
        options: ["TCP","UDP","ICMP","ARP"],
        answer: 2,
        explanation: "Perintah ping menggunakan protokol ICMP (Internet Control Message Protocol) untuk mengirim echo request dan menerima echo reply, digunakan untuk mengecek konektivitas jaringan."
      }
    ]
  }
];

// ============================================================
// STATE
// ============================================================
let state = {
  score: 0,
  streak: 0,
  correct: 0,
  currentTopic: null,
  currentQ: 0,
  answered: false,
  totalAnswered: 0
};

// ============================================================
// INIT MENU
// ============================================================
function initMenu() {
  const grid = document.getElementById('menuGrid');
  grid.innerHTML = '';
  TOPICS.forEach(topic => {
    const card = document.createElement('div');
    card.className = 'topic-card';
    card.onclick = () => startTopic(topic.id);
    card.innerHTML = `
      <span class="topic-icon">${topic.icon}</span>
      <div class="topic-name">${topic.name}</div>
      <div class="topic-desc">${topic.desc}</div>
      <div class="topic-count">${topic.questions.length} soal</div>
    `;
    card.style.setProperty('--topic-color', topic.color);
    grid.appendChild(card);
  });
}

// ============================================================
// START TOPIC
// ============================================================
function startTopic(topicId) {
  state.currentTopic = TOPICS.find(t => t.id === topicId);
  state.currentQ = 0;
  state.answered = false;
  showScreen('screenQuestion');
  renderQuestion();
}

function renderQuestion() {
  const topic = state.currentTopic;
  const q = topic.questions[state.currentQ];
  const total = topic.questions.length;

  // Header
  document.getElementById('qTopicTag').textContent = topic.name.toUpperCase();
  document.getElementById('qNumber').textContent = `Soal ${state.currentQ + 1} / ${total}`;

  // Progress
  const pct = Math.round((state.currentQ / total) * 100);
  document.getElementById('progressFill').style.width = pct + '%';
  document.getElementById('progressPct').textContent = pct + '%';
  document.getElementById('progressLabel').textContent = `${topic.name} — ${state.currentQ}/${total} selesai`;
  document.getElementById('questionDisplay').textContent = `${state.currentQ + 1}/${total}`;

  // Question text
  document.getElementById('questionText').textContent = q.q;

  // Visual
  const vis = document.getElementById('questionVisual');
  if (q.visual) {
    vis.style.display = 'block';
    vis.textContent = q.visual;
  } else {
    vis.style.display = 'none';
  }

  // Options
  const grid = document.getElementById('optionsGrid');
  grid.innerHTML = '';
  const letters = ['A', 'B', 'C', 'D'];
  q.options.forEach((opt, i) => {
    const btn = document.createElement('button');
    btn.className = 'option-btn';
    btn.innerHTML = `<span class="option-letter">${letters[i]}</span> ${opt}`;
    btn.onclick = () => selectAnswer(i);
    grid.appendChild(btn);
  });

  // Feedback
  document.getElementById('feedbackBox').className = 'feedback-box';
  document.getElementById('btnNext').style.display = 'none';
  state.answered = false;
}

// ============================================================
// SELECT ANSWER
// ============================================================
function selectAnswer(idx) {
  if (state.answered) return;
  state.answered = true;
  state.totalAnswered++;

  const topic = state.currentTopic;
  const q = topic.questions[state.currentQ];
  const btns = document.querySelectorAll('.option-btn');
  const isCorrect = (idx === q.answer);

  btns.forEach(b => b.disabled = true);
  btns[q.answer].classList.add('correct');
  if (!isCorrect) btns[idx].classList.add('wrong');

  const fb = document.getElementById('feedbackBox');
  const title = document.getElementById('feedbackTitle');
  const text = document.getElementById('feedbackText');

  if (isCorrect) {
    state.score += 10 + (state.streak >= 2 ? state.streak * 5 : 0);
    state.streak++;
    state.correct++;
    fb.className = 'feedback-box correct';
    title.textContent = '✓ BENAR!';
    text.textContent = q.explanation;
    updateScore();
    if (state.streak >= 3) showCombo(state.streak);
  } else {
    state.streak = 0;
    fb.className = 'feedback-box wrong';
    title.textContent = '✗ SALAH';
    text.textContent = q.explanation;
    updateScore();
  }

  document.getElementById('btnNext').style.display = 'inline-flex';
}

// ============================================================
// NEXT QUESTION
// ============================================================
function nextQuestion() {
  state.currentQ++;
  if (state.currentQ >= state.currentTopic.questions.length) {
    showResult();
  } else {
    renderQuestion();
  }
}

// ============================================================
// RESULT
// ============================================================
function showResult() {
  showScreen('screenResult');
  const total = state.currentTopic.questions.length;
  const pct = Math.round((state.correct / total) * 100);

  document.getElementById('resultPct').innerHTML = pct + '%<span>' + state.correct + '/' + total + ' benar</span>';

  let grade, msg;
  if (pct >= 90) { grade = 'S'; msg = '🏆 LUAR BIASA! Kamu master TKJ!'; }
  else if (pct >= 80) { grade = 'A'; msg = '⭐ Sangat bagus! Pemahaman sangat baik.'; }
  else if (pct >= 70) { grade = 'B'; msg = '👍 Bagus! Terus tingkatkan pemahamanmu.'; }
  else if (pct >= 60) { grade = 'C'; msg = '📚 Cukup. Pelajari lagi materi ini ya!'; }
  else { grade = 'D'; msg = '💪 Jangan menyerah! Ulangi dan pasti bisa!'; }

  const gradeEl = document.getElementById('resultGrade');
  gradeEl.textContent = 'GRADE ' + grade;
  gradeEl.className = 'result-grade ' + grade;
  document.getElementById('resultMessage').textContent = msg;
  document.getElementById('statCorrect').textContent = state.correct;
  document.getElementById('statWrong').textContent = total - state.correct;
  document.getElementById('statScore').textContent = state.score;

  document.getElementById('progressFill').style.width = '100%';
  document.getElementById('progressPct').textContent = '100%';
}

// ============================================================
// HELPERS
// ============================================================
function updateScore() {
  const sEl = document.getElementById('scoreDisplay');
  sEl.textContent = state.score;
  sEl.classList.remove('pulse');
  void sEl.offsetWidth;
  sEl.classList.add('pulse');
  document.getElementById('streakDisplay').textContent = state.streak + '🔥';
  document.getElementById('correctDisplay').textContent = state.correct;
}

function showCombo(streak) {
  const el = document.createElement('div');
  el.className = 'combo-badge';
  el.textContent = streak + 'x COMBO!';
  document.body.appendChild(el);
  setTimeout(() => el.remove(), 1200);
}

function showScreen(id) {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
}

function goMenu() {
  showScreen('screenMenu');
  document.getElementById('progressFill').style.width = '0%';
  document.getElementById('progressPct').textContent = '0%';
  document.getElementById('progressLabel').textContent = 'Pilih topik untuk memulai';
  document.getElementById('questionDisplay').textContent = '-';
  state.currentTopic = null;
  state.currentQ = 0;
  state.correct = 0;
}

function retryTopic() {
  const topicId = state.currentTopic.id;
  state.correct = 0;
  startTopic(topicId);
}

// Init
initMenu();
</script>
</body>
</html>
