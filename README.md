<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Vinay Kumar Mandadi — Portfolio README</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:ital,wght@0,400;0,700;1,400&family=Syne:wght@400;700;800;900&family=JetBrains+Mono:wght@300;400;600&display=swap" rel="stylesheet"/>
<style>
  :root {
    --gcp: #4285F4;
    --aws: #FF9900;
    --oracle: #F80000;
    --accent: #00F5D4;
    --accent2: #FFE156;
    --bg: #070B14;
    --bg2: #0D1526;
    --bg3: #111C35;
    --text: #E8EFF8;
    --muted: #6B84A8;
    --card: rgba(255,255,255,0.04);
    --border: rgba(255,255,255,0.07);
    --glow: rgba(66,133,244,0.25);
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'JetBrains Mono', monospace;
    overflow-x: hidden;
    cursor: crosshair;
  }

  /* ── CANVAS STARFIELD ── */
  #starfield {
    position: fixed; inset: 0; z-index: 0; pointer-events: none;
  }

  /* ── NOISE OVERLAY ── */
  body::before {
    content: '';
    position: fixed; inset: 0; z-index: 1; pointer-events: none;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.04'/%3E%3C/svg%3E");
    opacity: 0.4;
  }

  .wrapper {
    position: relative; z-index: 2;
    max-width: 900px;
    margin: 0 auto;
    padding: 0 24px 80px;
  }

  /* ── HERO ── */
  .hero {
    min-height: 100vh;
    display: flex; flex-direction: column;
    justify-content: center; align-items: flex-start;
    padding: 80px 0 40px;
    position: relative;
  }

  .hero-eyebrow {
    font-family: 'Space Mono', monospace;
    font-size: 11px;
    letter-spacing: 0.3em;
    color: var(--accent);
    text-transform: uppercase;
    margin-bottom: 18px;
    opacity: 0;
    animation: fadeUp 0.7s 0.2s forwards;
    display: flex; align-items: center; gap: 12px;
  }
  .hero-eyebrow::before {
    content: '';
    display: block; width: 40px; height: 1px;
    background: var(--accent);
  }

  .hero-name {
    font-family: 'Syne', sans-serif;
    font-weight: 900;
    font-size: clamp(52px, 9vw, 100px);
    line-height: 0.9;
    letter-spacing: -0.03em;
    margin-bottom: 24px;
    opacity: 0;
    animation: fadeUp 0.8s 0.4s forwards;
  }

  .hero-name span {
    display: block;
  }

  .hero-name .first { color: var(--text); }
  .hero-name .last {
    -webkit-text-stroke: 1.5px var(--gcp);
    color: transparent;
    position: relative;
  }
  .hero-name .last::after {
    content: 'MANDADI';
    position: absolute; left: 0; top: 0;
    color: var(--gcp);
    clip-path: inset(0 100% 0 0);
    animation: revealText 1.2s 1.2s cubic-bezier(.77,0,.18,1) forwards;
    -webkit-text-stroke: 0;
  }

  @keyframes revealText {
    to { clip-path: inset(0 0% 0 0); }
  }

  .hero-roles {
    display: flex; flex-wrap: wrap; gap: 8px;
    margin-bottom: 32px;
    opacity: 0;
    animation: fadeUp 0.7s 0.8s forwards;
  }

  .role-tag {
    font-family: 'Space Mono', monospace;
    font-size: 10px;
    letter-spacing: 0.15em;
    padding: 6px 14px;
    border: 1px solid var(--border);
    border-radius: 2px;
    background: var(--card);
    text-transform: uppercase;
    transition: all 0.2s;
  }
  .role-tag:hover {
    border-color: var(--accent);
    color: var(--accent);
    transform: translateY(-2px);
  }
  .role-tag.blue { border-color: rgba(66,133,244,0.4); color: #7EB8FF; }
  .role-tag.orange { border-color: rgba(255,153,0,0.4); color: #FFBA52; }
  .role-tag.red { border-color: rgba(248,0,0,0.35); color: #FF6B6B; }
  .role-tag.teal { border-color: rgba(0,245,212,0.4); color: var(--accent); }

  .hero-meta {
    font-size: 12px;
    color: var(--muted);
    line-height: 1.9;
    opacity: 0;
    animation: fadeUp 0.7s 1s forwards;
  }
  .hero-meta strong { color: var(--accent2); }

  .hero-links {
    display: flex; gap: 16px; margin-top: 32px;
    opacity: 0;
    animation: fadeUp 0.7s 1.2s forwards;
  }

  .hero-link {
    display: flex; align-items: center; gap: 8px;
    font-family: 'Space Mono', monospace;
    font-size: 11px;
    letter-spacing: 0.1em;
    color: var(--text);
    text-decoration: none;
    padding: 10px 20px;
    border: 1px solid var(--border);
    border-radius: 2px;
    transition: all 0.25s;
    background: var(--card);
  }
  .hero-link:hover {
    border-color: var(--gcp);
    box-shadow: 0 0 20px var(--glow);
    transform: translateY(-2px);
  }

  .scroll-indicator {
    position: absolute; bottom: 40px; left: 0;
    display: flex; align-items: center; gap: 12px;
    font-family: 'Space Mono', monospace;
    font-size: 10px;
    letter-spacing: 0.2em;
    color: var(--muted);
    text-transform: uppercase;
    opacity: 0;
    animation: fadeUp 0.6s 1.8s forwards;
  }
  .scroll-dot {
    width: 6px; height: 6px; border-radius: 50%;
    background: var(--accent);
    animation: pulse 2s infinite;
  }

  /* ── SECTION STYLES ── */
  section {
    padding: 80px 0;
    border-top: 1px solid var(--border);
  }

  .section-label {
    font-family: 'Space Mono', monospace;
    font-size: 10px;
    letter-spacing: 0.35em;
    color: var(--accent);
    text-transform: uppercase;
    margin-bottom: 12px;
    display: flex; align-items: center; gap: 12px;
  }
  .section-label::after {
    content: '';
    flex: 1; height: 1px;
    background: linear-gradient(to right, var(--border), transparent);
  }

  .section-title {
    font-family: 'Syne', sans-serif;
    font-weight: 800;
    font-size: clamp(28px, 4vw, 48px);
    line-height: 1.1;
    letter-spacing: -0.02em;
    margin-bottom: 40px;
  }

  /* ── ABOUT GRID ── */
  .about-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2px;
  }

  .about-cell {
    background: var(--card);
    border: 1px solid var(--border);
    padding: 28px;
    transition: all 0.3s;
    position: relative;
    overflow: hidden;
  }
  .about-cell::before {
    content: '';
    position: absolute; top: 0; left: 0;
    width: 3px; height: 0;
    background: var(--accent);
    transition: height 0.4s;
  }
  .about-cell:hover::before { height: 100%; }
  .about-cell:hover { background: rgba(255,255,255,0.06); }

  .about-cell-label {
    font-size: 10px;
    letter-spacing: 0.2em;
    color: var(--muted);
    text-transform: uppercase;
    margin-bottom: 10px;
  }
  .about-cell-value {
    font-family: 'Syne', sans-serif;
    font-weight: 700;
    font-size: 15px;
    line-height: 1.5;
  }
  .about-cell-value .hi { color: var(--accent2); }

  /* ── SKILLS GRID ── */
  .skills-group { margin-bottom: 36px; }

  .skills-group-title {
    font-size: 11px;
    letter-spacing: 0.2em;
    color: var(--muted);
    text-transform: uppercase;
    margin-bottom: 16px;
    padding-bottom: 8px;
    border-bottom: 1px solid var(--border);
  }

  .skills-tags {
    display: flex; flex-wrap: wrap; gap: 8px;
  }

  .skill-badge {
    font-size: 11px;
    padding: 7px 14px;
    border-radius: 2px;
    border: 1px solid var(--border);
    background: var(--card);
    transition: all 0.25s;
    cursor: default;
    position: relative;
    overflow: hidden;
  }
  .skill-badge::after {
    content: '';
    position: absolute; inset: 0;
    background: currentColor;
    opacity: 0;
    transition: opacity 0.25s;
  }
  .skill-badge:hover { transform: translateY(-2px) scale(1.03); }
  .skill-badge:hover::after { opacity: 0.08; }
  .skill-badge.cloud { color: #7EB8FF; border-color: rgba(66,133,244,0.35); }
  .skill-badge.devops { color: #A78BFA; border-color: rgba(167,139,250,0.35); }
  .skill-badge.lang { color: #FDE68A; border-color: rgba(253,230,138,0.35); }
  .skill-badge.ai { color: #F9A8D4; border-color: rgba(249,168,212,0.35); }

  /* ── PROJECTS ── */
  .project-card {
    border: 1px solid var(--border);
    background: var(--card);
    margin-bottom: 16px;
    overflow: hidden;
    transition: all 0.35s;
    position: relative;
  }
  .project-card::before {
    content: '';
    position: absolute; top: 0; left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, var(--gcp), var(--accent));
    transform: scaleX(0);
    transform-origin: left;
    transition: transform 0.4s;
  }
  .project-card:hover::before { transform: scaleX(1); }
  .project-card:hover {
    border-color: rgba(66,133,244,0.3);
    transform: translateX(4px);
    box-shadow: -4px 0 0 var(--gcp), 0 20px 40px rgba(0,0,0,0.4);
  }

  .project-header {
    display: flex; align-items: center;
    justify-content: space-between;
    padding: 24px 28px 16px;
    cursor: pointer;
  }

  .project-title-row {
    display: flex; align-items: center; gap: 14px;
  }

  .project-icon {
    font-size: 22px;
    width: 44px; height: 44px;
    display: flex; align-items: center; justify-content: center;
    background: rgba(255,255,255,0.05);
    border: 1px solid var(--border);
    border-radius: 4px;
    flex-shrink: 0;
  }

  .project-name {
    font-family: 'Syne', sans-serif;
    font-weight: 700;
    font-size: 17px;
    letter-spacing: -0.01em;
  }
  .project-sub {
    font-size: 10px;
    color: var(--muted);
    margin-top: 3px;
  }

  .expand-btn {
    font-size: 18px;
    color: var(--muted);
    transition: transform 0.3s, color 0.3s;
    background: none; border: none; cursor: pointer;
    color: var(--muted);
  }

  .project-body {
    max-height: 0; overflow: hidden;
    transition: max-height 0.5s cubic-bezier(.4,0,.2,1);
  }
  .project-body.open { max-height: 600px; }

  .project-content {
    padding: 0 28px 28px;
    border-top: 1px solid var(--border);
    padding-top: 20px;
  }

  .project-tags {
    display: flex; flex-wrap: wrap; gap: 6px;
    margin-bottom: 14px;
  }
  .project-tag {
    font-size: 9px;
    letter-spacing: 0.15em;
    padding: 3px 10px;
    border-radius: 2px;
    background: rgba(255,255,255,0.06);
    border: 1px solid var(--border);
    text-transform: uppercase;
    color: var(--accent);
  }

  .project-desc {
    font-size: 12px;
    line-height: 1.9;
    color: #A8BDD4;
  }

  .impact-stat {
    display: inline-block;
    font-family: 'Syne', sans-serif;
    font-weight: 800;
    color: var(--accent2);
    font-size: 18px;
  }

  /* ── CERTS ── */
  .certs-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 8px;
  }

  .cert-item {
    display: flex; align-items: center; gap: 14px;
    padding: 16px 20px;
    border: 1px solid var(--border);
    background: var(--card);
    border-radius: 2px;
    transition: all 0.25s;
  }
  .cert-item:hover {
    border-color: var(--accent);
    transform: translateY(-2px);
    box-shadow: 0 8px 24px rgba(0,0,0,0.3);
  }
  .cert-icon { font-size: 22px; flex-shrink: 0; }
  .cert-name {
    font-size: 11px;
    line-height: 1.5;
    color: #C0D4F0;
  }

  /* ── ACHIEVEMENTS ── */
  .achievement-row {
    display: flex; align-items: flex-start; gap: 20px;
    padding: 24px;
    border: 1px solid var(--border);
    background: var(--card);
    margin-bottom: 8px;
    transition: all 0.25s;
    position: relative;
    overflow: hidden;
  }
  .achievement-row::after {
    content: '';
    position: absolute; right: -60px; top: -60px;
    width: 120px; height: 120px;
    border-radius: 50%;
    background: radial-gradient(circle, rgba(255,225,86,0.08), transparent);
    pointer-events: none;
  }
  .achievement-row:hover {
    border-color: rgba(255,225,86,0.3);
    transform: translateX(4px);
  }

  .achievement-num {
    font-family: 'Syne', sans-serif;
    font-weight: 900;
    font-size: 36px;
    line-height: 1;
    color: var(--accent2);
    flex-shrink: 0;
    min-width: 60px;
  }

  .achievement-text {
    font-size: 12px;
    line-height: 1.8;
    color: #A8BDD4;
    padding-top: 4px;
  }
  .achievement-text strong { color: var(--text); }

  /* ── FOOTER ── */
  .footer {
    border-top: 1px solid var(--border);
    padding: 40px 0 20px;
    display: flex; align-items: center; justify-content: space-between;
    flex-wrap: wrap; gap: 16px;
  }

  .footer-left {
    font-family: 'Space Mono', monospace;
    font-size: 11px;
    color: var(--muted);
  }

  .footer-right {
    display: flex; gap: 12px;
  }

  .footer-lang {
    font-size: 11px;
    padding: 5px 12px;
    border: 1px solid var(--border);
    border-radius: 2px;
    background: var(--card);
    color: var(--muted);
    transition: all 0.2s;
  }
  .footer-lang:hover {
    color: var(--accent);
    border-color: var(--accent);
  }

  /* ── TERMINAL TICKER ── */
  .ticker {
    position: fixed; bottom: 0; left: 0; right: 0;
    z-index: 100;
    background: rgba(7,11,20,0.9);
    backdrop-filter: blur(10px);
    border-top: 1px solid var(--border);
    padding: 8px 0;
    overflow: hidden;
  }

  .ticker-inner {
    display: flex; gap: 0;
    animation: scrollTicker 30s linear infinite;
    white-space: nowrap;
  }

  .ticker-item {
    font-family: 'Space Mono', monospace;
    font-size: 11px;
    color: var(--muted);
    padding: 0 40px;
    display: flex; align-items: center; gap: 10px;
  }
  .ticker-item .dot { color: var(--accent); }

  @keyframes scrollTicker {
    0% { transform: translateX(0); }
    100% { transform: translateX(-50%); }
  }

  /* ── UTILITIES ── */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 1; transform: translateY(0); }
  }

  @keyframes pulse {
    0%, 100% { opacity: 1; transform: scale(1); }
    50% { opacity: 0.4; transform: scale(0.8); }
  }

  .reveal {
    opacity: 0; transform: translateY(24px);
    transition: opacity 0.7s, transform 0.7s;
  }
  .reveal.visible {
    opacity: 1; transform: translateY(0);
  }

  .glow-line {
    height: 1px;
    background: linear-gradient(to right, transparent, var(--gcp), var(--accent), transparent);
    margin: 0 0 60px;
    opacity: 0.5;
  }

  /* ── LIVE STATUS ── */
  .status-bar {
    display: flex; align-items: center; gap: 8px;
    margin-bottom: 20px;
    opacity: 0;
    animation: fadeUp 0.6s 1.5s forwards;
  }
  .status-dot {
    width: 8px; height: 8px; border-radius: 50%;
    background: #22C55E;
    box-shadow: 0 0 8px #22C55E;
    animation: pulse 2s infinite;
  }
  .status-text {
    font-size: 11px;
    color: #22C55E;
    letter-spacing: 0.1em;
    font-family: 'Space Mono', monospace;
  }

  /* ── RESPONSIVE ── */
  @media (max-width: 600px) {
    .about-grid { grid-template-columns: 1fr; }
    .certs-grid { grid-template-columns: 1fr; }
    .hero-name { font-size: 52px; }
  }
</style>
</head>
<body>

<canvas id="starfield"></canvas>

<!-- TICKER -->
<div class="ticker">
  <div class="ticker-inner" id="ticker-inner">
    <span class="ticker-item"><span class="dot">▸</span> Oracle Cloud Multicloud Architect</span>
    <span class="ticker-item"><span class="dot">▸</span> GCP Gold Tier</span>
    <span class="ticker-item"><span class="dot">▸</span> GRE 324 · Quant 168</span>
    <span class="ticker-item"><span class="dot">▸</span> GDG Solution Challenge · Top 3% Nationally</span>
    <span class="ticker-item"><span class="dot">▸</span> Deepfake Detection · 97–99% AUC</span>
    <span class="ticker-item"><span class="dot">▸</span> No. 3 Globally · Knives Out Season 47</span>
    <span class="ticker-item"><span class="dot">▸</span> 200+ DSA Problems · LeetCode + GfG</span>
    <span class="ticker-item"><span class="dot">▸</span> Edge AI · MLOps · IaC Specialist</span>
    <!-- duplicate for seamless loop -->
    <span class="ticker-item"><span class="dot">▸</span> Oracle Cloud Multicloud Architect</span>
    <span class="ticker-item"><span class="dot">▸</span> GCP Gold Tier</span>
    <span class="ticker-item"><span class="dot">▸</span> GRE 324 · Quant 168</span>
    <span class="ticker-item"><span class="dot">▸</span> GDG Solution Challenge · Top 3% Nationally</span>
    <span class="ticker-item"><span class="dot">▸</span> Deepfake Detection · 97–99% AUC</span>
    <span class="ticker-item"><span class="dot">▸</span> No. 3 Globally · Knives Out Season 47</span>
    <span class="ticker-item"><span class="dot">▸</span> 200+ DSA Problems · LeetCode + GfG</span>
    <span class="ticker-item"><span class="dot">▸</span> Edge AI · MLOps · IaC Specialist</span>
  </div>
</div>

<div class="wrapper">

  <!-- ── HERO ── -->
  <section class="hero" style="border: none; padding-top: 80px;">
    <div class="hero-eyebrow">Full-Stack Cloud & AI Portfolio</div>
    <div class="hero-name">
      <span class="first">VINAY KUMAR</span>
      <span class="last">MANDADI</span>
    </div>
    <div class="hero-roles">
      <span class="role-tag blue">☁️ Multicloud Architect</span>
      <span class="role-tag orange">⚙️ DevOps Engineer</span>
      <span class="role-tag red">🧠 MLOps Specialist</span>
      <span class="role-tag teal">🔬 Edge AI Researcher</span>
    </div>
    <div class="status-bar">
      <div class="status-dot"></div>
      <span class="status-text">AVAILABLE · Final-Year B.Tech CSE @ LPU · GRE 324</span>
    </div>
    <div class="hero-meta">
      <strong>📍 Lovely Professional University</strong> &nbsp;·&nbsp; mvkchowdary20@gmail.com<br/>
      Automating distributed multicloud architectures &amp; fault-tolerant CD pipelines.<br/>
      Passionate about <strong>IaC × Edge AI</strong> — deploying generative models to edge without latency.
    </div>
    <div class="hero-links">
      <a href="https://linkedin.com/in/vinay-kumar-chowdary" class="hero-link">
        <svg width="14" height="14" viewBox="0 0 24 24" fill="currentColor"><path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6zM2 9h4v12H2z"/><circle cx="4" cy="4" r="2"/></svg>
        LinkedIn
      </a>
      <a href="mailto:mvkchowdary20@gmail.com" class="hero-link">
        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/><polyline points="22,6 12,13 2,6"/></svg>
        Email
      </a>
      <a href="https://github.com/vinaykumarchowdary18" class="hero-link">
        <svg width="14" height="14" viewBox="0 0 24 24" fill="currentColor"><path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/></svg>
        GitHub
      </a>
    </div>
    <div class="scroll-indicator">
      <div class="scroll-dot"></div>
      Scroll to explore
    </div>
  </section>

  <div class="glow-line"></div>

  <!-- ── ABOUT ── -->
  <section class="reveal">
    <div class="section-label">01 — Profile</div>
    <h2 class="section-title">Global Mindset.<br/>Technical Edge.</h2>
    <div class="about-grid">
      <div class="about-cell">
        <div class="about-cell-label">Academic Focus</div>
        <div class="about-cell-value">
          Virtualization &amp; Cloud Computing<br/>
          Site Reliability Engineering<br/>
          <span class="hi">Cloud Capstone · TGPA 7.0+</span>
        </div>
      </div>
      <div class="about-cell">
        <div class="about-cell-label">Languages Spoken</div>
        <div class="about-cell-value">
          🇬🇧 English (Fluent) · 🇮🇳 Telugu (Native)<br/>
          🇮🇳 Hindi (Fluent)<br/>
          <span class="hi">🇯🇵 Japanese JLPT N4 · 🇨🇳 Chinese HSK 1</span>
        </div>
      </div>
      <div class="about-cell">
        <div class="about-cell-label">Beyond the Screen</div>
        <div class="about-cell-value">
          ✈️ Aviation enthusiast &amp; frequent flyer<br/>
          🎮 <span class="hi">Ranked #3 Globally</span> · Knives Out<br/>
          Legendary Mars · Season 47
        </div>
      </div>
      <div class="about-cell">
        <div class="about-cell-label">Research</div>
        <div class="about-cell-value">
          Working Paper: <span class="hi">AIRIMF</span><br/>
          AI-Based Risk Identification &amp;<br/>
          Mitigation Framework
        </div>
      </div>
    </div>
  </section>

  <!-- ── SKILLS ── -->
  <section class="reveal">
    <div class="section-label">02 — Tech Stack</div>
    <h2 class="section-title">Comprehensive<br/>Toolkit.</h2>

    <div class="skills-group">
      <div class="skills-group-title">☁️ Cloud &amp; Infrastructure</div>
      <div class="skills-tags">
        <span class="skill-badge cloud">Google Cloud</span>
        <span class="skill-badge cloud">AWS</span>
        <span class="skill-badge cloud">Oracle Cloud</span>
        <span class="skill-badge cloud">Firebase</span>
        <span class="skill-badge cloud">Linux</span>
      </div>
    </div>

    <div class="skills-group">
      <div class="skills-group-title">⚙️ DevOps &amp; CI/CD</div>
      <div class="skills-tags">
        <span class="skill-badge devops">Terraform</span>
        <span class="skill-badge devops">Docker</span>
        <span class="skill-badge devops">Kubernetes</span>
        <span class="skill-badge devops">Git</span>
        <span class="skill-badge devops">Bash Scripting</span>
      </div>
    </div>

    <div class="skills-group">
      <div class="skills-group-title">💻 Languages &amp; Frameworks</div>
      <div class="skills-tags">
        <span class="skill-badge lang">C++</span>
        <span class="skill-badge lang">Python</span>
        <span class="skill-badge lang">Java</span>
        <span class="skill-badge lang">JavaScript</span>
        <span class="skill-badge lang">Node.js</span>
        <span class="skill-badge lang">React</span>
        <span class="skill-badge lang">Flask</span>
        <span class="skill-badge lang">Flutter</span>
      </div>
    </div>

    <div class="skills-group">
      <div class="skills-group-title">🧠 AI/ML &amp; Databases</div>
      <div class="skills-tags">
        <span class="skill-badge ai">TensorFlow</span>
        <span class="skill-badge ai">PyTorch</span>
        <span class="skill-badge ai">OpenCV</span>
        <span class="skill-badge ai">ONNX</span>
        <span class="skill-badge ai">TensorRT</span>
        <span class="skill-badge ai">MySQL</span>
        <span class="skill-badge ai">MongoDB</span>
      </div>
    </div>
  </section>

  <!-- ── PROJECTS ── -->
  <section class="reveal">
    <div class="section-label">03 — Portfolio</div>
    <h2 class="section-title">Architecture &amp;<br/>Deployment Work.</h2>

    <!-- Project 1 -->
    <div class="project-card">
      <div class="project-header" onclick="toggleProject(this)">
        <div class="project-title-row">
          <div class="project-icon">🕵️</div>
          <div>
            <div class="project-name">Truth In Pixels</div>
            <div class="project-sub">Dual-Branch Deepfake Detection · Capstone Project</div>
          </div>
        </div>
        <button class="expand-btn">+</button>
      </div>
      <div class="project-body">
        <div class="project-content">
          <div class="project-tags">
            <span class="project-tag">Python</span>
            <span class="project-tag">PyTorch</span>
            <span class="project-tag">Flask</span>
            <span class="project-tag">ONNX</span>
            <span class="project-tag">TensorRT</span>
            <span class="project-tag">GradCAM</span>
          </div>
          <div class="project-desc">
            EfficientNetV2-M (RGB Spatial) + CNN (DCT Frequency) dual-branch pipeline for detecting GAN artifacts. Achieved <span class="impact-stat">97–99%</span> AUC on FaceForensics++. Exported to ONNX for TensorRT compatibility ensuring high-speed inference. Flask web app with SQLite for real-time video upload and XAI heatmap visualizations.
          </div>
        </div>
      </div>
    </div>

    <!-- Project 2 -->
    <div class="project-card">
      <div class="project-header" onclick="toggleProject(this)">
        <div class="project-title-row">
          <div class="project-icon">🗳️</div>
          <div>
            <div class="project-name">Secure Aadhaar Voting System</div>
            <div class="project-sub">Cloud-Native Voting Platform on GCP</div>
          </div>
        </div>
        <button class="expand-btn">+</button>
      </div>
      <div class="project-body">
        <div class="project-content">
          <div class="project-tags">
            <span class="project-tag">GCP</span>
            <span class="project-tag">Node.js</span>
            <span class="project-tag">MySQL</span>
            <span class="project-tag">Cloud Storage</span>
            <span class="project-tag">IAM</span>
          </div>
          <div class="project-desc">
            Architected a cloud-native voting platform with static frontend on GCP Cloud Storage. Secured backend via Cloud SQL with IAM database authentication. Reduced voter verification latency by <span class="impact-stat">80%</span> through structural optimizations.
          </div>
        </div>
      </div>
    </div>

    <!-- Project 3 -->
    <div class="project-card">
      <div class="project-header" onclick="toggleProject(this)">
        <div class="project-title-row">
          <div class="project-icon">🧭</div>
          <div>
            <div class="project-name">Compass AI Travel Planner</div>
            <div class="project-sub">Cross-Platform Mobile App with AI Itineraries</div>
          </div>
        </div>
        <button class="expand-btn">+</button>
      </div>
      <div class="project-body">
        <div class="project-content">
          <div class="project-tags">
            <span class="project-tag">Flutter</span>
            <span class="project-tag">Firebase</span>
            <span class="project-tag">Data Connect</span>
            <span class="project-tag">flutter_map</span>
          </div>
          <div class="project-desc">
            Cross-platform mobile app with AI-generated itineraries. Integrated flutter_map for real-time location mapping and configured local emulators for seamless backend data flow testing.
          </div>
        </div>
      </div>
    </div>

    <!-- Project 4 -->
    <div class="project-card">
      <div class="project-header" onclick="toggleProject(this)">
        <div class="project-title-row">
          <div class="project-icon">🚗</div>
          <div>
            <div class="project-name">EcoRide Carpooling System</div>
            <div class="project-sub">Full-Stack MEAN Application</div>
          </div>
        </div>
        <button class="expand-btn">+</button>
      </div>
      <div class="project-body">
        <div class="project-content">
          <div class="project-tags">
            <span class="project-tag">MongoDB</span>
            <span class="project-tag">Express.js</span>
            <span class="project-tag">React</span>
            <span class="project-tag">Node.js</span>
            <span class="project-tag">bcrypt</span>
          </div>
          <div class="project-desc">
            Complete MEAN stack ride-sharing platform. Engineered data flow from Node.js backend to MongoDB. Implemented bcrypt encryption for robust user authentication and ride scheduling.
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- ── CERTIFICATIONS ── -->
  <section class="reveal">
    <div class="section-label">04 — Credentials</div>
    <h2 class="section-title">Official<br/>Certifications.</h2>
    <div class="certs-grid">
      <div class="cert-item"><div class="cert-icon">🏆</div><div class="cert-name">Oracle Cloud Infrastructure<br/><strong>Multicloud Architect</strong></div></div>
      <div class="cert-item"><div class="cert-icon">🏆</div><div class="cert-name">Oracle Cloud Infrastructure<br/><strong>DevOps Professional</strong></div></div>
      <div class="cert-item"><div class="cert-icon">🏆</div><div class="cert-name">Oracle Cloud Infrastructure<br/><strong>AI Foundations Associate</strong></div></div>
      <div class="cert-item"><div class="cert-icon">🏆</div><div class="cert-name">Oracle Cloud<br/><strong>Data Management Foundations</strong></div></div>
      <div class="cert-item"><div class="cert-icon">☁️</div><div class="cert-name">Google Cloud<br/><strong>Data Analytics Certificate</strong></div></div>
      <div class="cert-item"><div class="cert-icon">☁️</div><div class="cert-name">AWS<br/><strong>Cloud Practitioner Essentials</strong></div></div>
      <div class="cert-item"><div class="cert-icon">🐳</div><div class="cert-name">Docker, Kubernetes &amp; OpenShift<br/><strong>Introduction to Containers</strong></div></div>
      <div class="cert-item"><div class="cert-icon">🐧</div><div class="cert-name">Linux Commands &amp; Shell<br/><strong>Hands-on Introduction</strong></div></div>
    </div>
  </section>

  <!-- ── ACHIEVEMENTS ── -->
  <section class="reveal">
    <div class="section-label">05 — Highlights</div>
    <h2 class="section-title">Research &amp;<br/>Global Achievements.</h2>

    <div class="achievement-row">
      <div class="achievement-num">3%</div>
      <div class="achievement-text">
        <strong>GDG Solution Challenge · National Finalist</strong><br/>
        Ranked in the Top 3% — Top 105 out of 3,700+ competing teams for innovation and impact.
      </div>
    </div>

    <div class="achievement-row">
      <div class="achievement-num">Gold</div>
      <div class="achievement-text">
        <strong>Google Cloud Skills · Gold Tier</strong><br/>
        Extensive completion of hands-on cloud labs and infrastructure quests across GCP.
      </div>
    </div>

    <div class="achievement-row">
      <div class="achievement-num">#3</div>
      <div class="achievement-text">
        <strong>Knives Out · Ranked No. 3 Globally</strong><br/>
        Legendary Mars tier in Season 47 — competitive gaming at a global elite level.
      </div>
    </div>

    <div class="achievement-row">
      <div class="achievement-num">200+</div>
      <div class="achievement-text">
        <strong>Algorithmic Problem Solving</strong><br/>
        Data Structures &amp; Algorithms problems conquered across LeetCode and GeeksForGeeks.
      </div>
    </div>

    <div class="achievement-row">
      <div class="achievement-num">324</div>
      <div class="achievement-text">
        <strong>GRE Score · Quant: 168/170</strong><br/>
        Strong quantitative aptitude demonstrating analytical and mathematical excellence.
      </div>
    </div>
  </section>

  <!-- ── FOOTER ── -->
  <footer class="footer reveal">
    <div class="footer-left">
      © 2025 Vinay Kumar Mandadi &nbsp;·&nbsp; Built with precision &amp; passion
    </div>
    <div class="footer-right">
      <span class="footer-lang">🇯🇵 JLPT N4</span>
      <span class="footer-lang">🇨🇳 HSK 1</span>
      <span class="footer-lang">🇮🇳 Telugu</span>
      <span class="footer-lang">🇬🇧 English</span>
    </div>
  </footer>

</div>

<script>
// ── STARFIELD CANVAS ──
const canvas = document.getElementById('starfield');
const ctx = canvas.getContext('2d');
let stars = [];
let W, H;

function resize() {
  W = canvas.width = window.innerWidth;
  H = canvas.height = window.innerHeight;
}

function initStars() {
  stars = [];
  for (let i = 0; i < 180; i++) {
    stars.push({
      x: Math.random() * W,
      y: Math.random() * H,
      r: Math.random() * 1.2 + 0.2,
      speed: Math.random() * 0.3 + 0.05,
      opacity: Math.random() * 0.6 + 0.1,
      twinkleSpeed: Math.random() * 0.02 + 0.005,
      twinkleOffset: Math.random() * Math.PI * 2
    });
  }
}

let frame = 0;
function animate() {
  ctx.clearRect(0, 0, W, H);
  frame += 0.016;
  stars.forEach(s => {
    const twinkle = Math.sin(frame * s.twinkleSpeed * 60 + s.twinkleOffset) * 0.3 + 0.7;
    ctx.beginPath();
    ctx.arc(s.x, s.y, s.r, 0, Math.PI * 2);
    ctx.fillStyle = `rgba(140, 190, 255, ${s.opacity * twinkle})`;
    ctx.fill();
    s.y -= s.speed;
    if (s.y < -2) { s.y = H + 2; s.x = Math.random() * W; }
  });
  requestAnimationFrame(animate);
}

resize();
initStars();
animate();
window.addEventListener('resize', () => { resize(); initStars(); });

// ── PROJECT TOGGLES ──
function toggleProject(header) {
  const card = header.parentElement;
  const body = card.querySelector('.project-body');
  const btn = header.querySelector('.expand-btn');
  const isOpen = body.classList.contains('open');
  document.querySelectorAll('.project-body.open').forEach(b => {
    b.classList.remove('open');
    b.previousElementSibling.querySelector('.expand-btn').textContent = '+';
  });
  if (!isOpen) {
    body.classList.add('open');
    btn.textContent = '−';
  }
}

// ── SCROLL REVEAL ──
const reveals = document.querySelectorAll('.reveal');
const observer = new IntersectionObserver(entries => {
  entries.forEach(e => {
    if (e.isIntersecting) {
      e.target.classList.add('visible');
    }
  });
}, { threshold: 0.1 });
reveals.forEach(el => observer.observe(el));

// ── CURSOR GLOW ──
document.addEventListener('mousemove', e => {
  const glow = document.getElementById('cursor-glow');
  if (glow) {
    glow.style.left = e.clientX + 'px';
    glow.style.top = e.clientY + 'px';
  }
});
</script>
</body>
</html>
