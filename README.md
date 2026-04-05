<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Harrison — ML Researcher</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Mono:ital,wght@0,300;0,400;0,500;1,300&family=Syne:wght@400;600;700;800&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #0a0a0f;
    --bg2: #111118;
    --bg3: #18181f;
    --border: rgba(255,255,255,0.07);
    --border2: rgba(255,255,255,0.12);
    --text: #e8e8f0;
    --muted: #8888a0;
    --accent: #7b6cff;
    --accent2: #4fc8a0;
    --accent3: #ff6b6b;
    --mono: 'DM Mono', monospace;
    --display: 'Syne', sans-serif;
  }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: var(--mono);
    font-size: 14px;
    line-height: 1.7;
    padding: 48px 24px 80px;
    max-width: 860px;
    margin: 0 auto;
    overflow-x: hidden;
  }

  /* ── Noise overlay ── */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.04'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 0;
    opacity: 0.4;
  }

  /* ── Grid lines ── */
  body::after {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(123,108,255,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(123,108,255,0.03) 1px, transparent 1px);
    background-size: 48px 48px;
    pointer-events: none;
    z-index: 0;
  }

  * { position: relative; z-index: 1; }

  /* ── Animations ── */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(20px); }
    to   { opacity: 1; transform: translateY(0); }
  }
  @keyframes fadeIn {
    from { opacity: 0; }
    to   { opacity: 1; }
  }
  @keyframes pulse {
    0%, 100% { opacity: 1; }
    50%       { opacity: 0.4; }
  }
  @keyframes scanline {
    0%   { transform: translateY(-100%); }
    100% { transform: translateY(100vh); }
  }
  @keyframes blink {
    0%, 100% { opacity: 1; }
    50%       { opacity: 0; }
  }
  @keyframes borderGlow {
    0%, 100% { border-color: rgba(123,108,255,0.2); }
    50%       { border-color: rgba(123,108,255,0.5); }
  }
  @keyframes countUp {
    from { opacity: 0; transform: translateY(8px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  .fade-up { animation: fadeUp 0.7s cubic-bezier(.22,.68,0,1.2) both; }
  .d1 { animation-delay: 0.1s; }
  .d2 { animation-delay: 0.2s; }
  .d3 { animation-delay: 0.35s; }
  .d4 { animation-delay: 0.5s; }
  .d5 { animation-delay: 0.65s; }
  .d6 { animation-delay: 0.8s; }
  .d7 { animation-delay: 0.95s; }
  .d8 { animation-delay: 1.1s; }

  /* ── Scanline effect ── */
  .scanline {
    position: fixed;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, transparent, rgba(123,108,255,0.15), transparent);
    animation: scanline 6s linear infinite;
    pointer-events: none;
    z-index: 999;
  }

  /* ── Header ── */
  .header { margin-bottom: 48px; }

  .status-bar {
    display: flex;
    align-items: center;
    gap: 8px;
    margin-bottom: 24px;
    font-size: 11px;
    color: var(--muted);
    letter-spacing: 0.08em;
  }
  .status-dot {
    width: 6px; height: 6px;
    border-radius: 50%;
    background: var(--accent2);
    animation: pulse 2s ease infinite;
  }

  .name {
    font-family: var(--display);
    font-size: clamp(42px, 8vw, 72px);
    font-weight: 800;
    line-height: 0.95;
    letter-spacing: -0.03em;
    margin-bottom: 16px;
  }
  .name span.accent { color: var(--accent); }
  .name span.accent2 { color: var(--accent2); }

  .tagline {
    font-size: 13px;
    color: var(--muted);
    letter-spacing: 0.04em;
    margin-bottom: 20px;
    max-width: 480px;
  }
  .tagline em {
    color: var(--text);
    font-style: normal;
  }

  .cursor {
    display: inline-block;
    width: 2px; height: 1em;
    background: var(--accent);
    margin-left: 2px;
    vertical-align: middle;
    animation: blink 1s step-end infinite;
  }

  /* ── Affiliation strip ── */
  .affiliations {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    margin-bottom: 40px;
  }
  .aff-tag {
    font-size: 11px;
    letter-spacing: 0.06em;
    padding: 5px 10px;
    border: 1px solid var(--border2);
    border-radius: 3px;
    color: var(--muted);
    background: var(--bg2);
    transition: border-color 0.2s, color 0.2s;
  }
  .aff-tag:hover { border-color: var(--accent); color: var(--text); }
  .aff-tag .dot { color: var(--accent2); margin-right: 5px; }

  /* ── Section label ── */
  .section-label {
    font-size: 10px;
    letter-spacing: 0.2em;
    color: var(--accent);
    text-transform: uppercase;
    margin-bottom: 16px;
    display: flex;
    align-items: center;
    gap: 10px;
  }
  .section-label::after {
    content: '';
    flex: 1;
    height: 1px;
    background: var(--border);
  }

  /* ── Research cards ── */
  .research-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
    gap: 12px;
    margin-bottom: 40px;
  }

  .r-card {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: 6px;
    padding: 20px;
    transition: border-color 0.25s, transform 0.25s;
    cursor: default;
  }
  .r-card:hover {
    border-color: var(--accent);
    transform: translateY(-2px);
  }
  .r-card.primary { border-color: rgba(123,108,255,0.3); animation: borderGlow 3s ease infinite; }

  .r-card-top {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 10px;
  }
  .r-venue {
    font-size: 10px;
    letter-spacing: 0.1em;
    color: var(--accent2);
    text-transform: uppercase;
  }
  .r-status {
    font-size: 10px;
    padding: 2px 7px;
    border-radius: 2px;
    letter-spacing: 0.06em;
  }
  .r-status.active { background: rgba(79,200,160,0.12); color: var(--accent2); }
  .r-status.incoming { background: rgba(123,108,255,0.12); color: var(--accent); }
  .r-status.pub { background: rgba(255,107,107,0.12); color: var(--accent3); }

  .r-title {
    font-family: var(--display);
    font-size: 15px;
    font-weight: 600;
    line-height: 1.3;
    margin-bottom: 6px;
    color: var(--text);
  }
  .r-desc {
    font-size: 12px;
    color: var(--muted);
    line-height: 1.6;
  }
  .r-pi {
    margin-top: 12px;
    font-size: 11px;
    color: var(--muted);
    letter-spacing: 0.04em;
  }
  .r-pi em { color: var(--text); font-style: normal; }

  /* ── Stats row ── */
  .stats-row {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
    margin-bottom: 40px;
  }
  .stat-box {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: 6px;
    padding: 16px 14px;
    text-align: center;
    animation: countUp 0.5s ease both;
  }
  .stat-num {
    font-family: var(--display);
    font-size: 28px;
    font-weight: 800;
    color: var(--accent);
    line-height: 1;
    margin-bottom: 4px;
  }
  .stat-label {
    font-size: 10px;
    color: var(--muted);
    letter-spacing: 0.08em;
    text-transform: uppercase;
  }

  /* ── Tech stack ── */
  .stack-grid {
    display: flex;
    flex-wrap: wrap;
    gap: 7px;
    margin-bottom: 40px;
  }
  .stack-tag {
    font-size: 11px;
    font-family: var(--mono);
    padding: 4px 10px;
    background: var(--bg3);
    border: 1px solid var(--border);
    border-radius: 3px;
    color: var(--muted);
    transition: all 0.2s;
  }
  .stack-tag:hover { color: var(--accent2); border-color: var(--accent2); }
  .stack-tag.hot { color: var(--accent); border-color: rgba(123,108,255,0.3); }

  /* ── Timeline ── */
  .timeline { margin-bottom: 40px; }
  .tl-item {
    display: grid;
    grid-template-columns: 120px 1fr;
    gap: 16px;
    padding: 14px 0;
    border-bottom: 1px solid var(--border);
    align-items: start;
  }
  .tl-item:last-child { border-bottom: none; }
  .tl-date {
    font-size: 11px;
    color: var(--muted);
    letter-spacing: 0.06em;
    padding-top: 2px;
  }
  .tl-content { }
  .tl-title {
    font-family: var(--display);
    font-size: 14px;
    font-weight: 600;
    margin-bottom: 2px;
  }
  .tl-sub { font-size: 12px; color: var(--muted); }

  /* ── Contact footer ── */
  .footer {
    margin-top: 56px;
    padding-top: 24px;
    border-top: 1px solid var(--border);
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    gap: 12px;
  }
  .footer-links { display: flex; gap: 16px; }
  .footer-link {
    font-size: 11px;
    color: var(--muted);
    text-decoration: none;
    letter-spacing: 0.06em;
    transition: color 0.2s;
  }
  .footer-link:hover { color: var(--accent); }
  .footer-note {
    font-size: 10px;
    color: rgba(136,136,160,0.5);
    letter-spacing: 0.06em;
  }

  /* ── Divider ── */
  .divider {
    height: 1px;
    background: var(--border);
    margin: 32px 0;
  }

  /* ── About block ── */
  .about {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-left: 3px solid var(--accent);
    border-radius: 0 6px 6px 0;
    padding: 20px 24px;
    margin-bottom: 40px;
    font-size: 13px;
    color: var(--muted);
    line-height: 1.8;
  }
  .about em { color: var(--text); font-style: normal; }

  @media (max-width: 600px) {
    .stats-row { grid-template-columns: repeat(2, 1fr); }
    .tl-item { grid-template-columns: 90px 1fr; }
    .footer { flex-direction: column; align-items: flex-start; }
  }
</style>
</head>
<body>

<div class="scanline"></div>

<!-- Header -->
<header class="header">
  <div class="status-bar fade-up d1">
    <span class="status-dot"></span>
    <span>UNC Chapel Hill · B.S. Computer Science &amp; Data Science · 2027</span>
  </div>

  <div class="name fade-up d2">
    Harrison<br><span class="accent">Lee</span><span class="cursor"></span>
  </div>

  <p class="tagline fade-up d3">
    Undergraduate ML researcher working at the intersection of
    <em>symbolic methods</em>, <em>neural ODEs</em>, and <em>computational biology</em> —
    building models that explain, not just predict.
  </p>

  <div class="affiliations fade-up d4">
    <span class="aff-tag"><span class="dot">▸</span>ACM Lab · UNC School of Medicine</span>
    <span class="aff-tag"><span class="dot">▸</span>Bai Lab · Cardiac Ultrasound</span>
    <span class="aff-tag"><span class="dot">▸</span>MD Anderson Cancer Center '26</span>
    <span class="aff-tag"><span class="dot">▸</span>UC Irvine · Choi Lab '26</span>
  </div>
</header>

<!-- About -->
<div class="fade-up d4">
  <div class="section-label">about</div>
  <div class="about">
    I'm a sophomore at UNC Chapel Hill pursuing dual degrees in CS and Data Science.
    My research sits at the boundary between <em>mechanistic modeling</em> and <em>deep learning</em> —
    I want to know why a model works, not just that it does.
    Current focus: symbolic regression for Alzheimer's tau propagation,
    cardiac ultrasound segmentation, and incoming work on ML for optical blood flow imaging.
    I'm building toward a research career at the frontier of ML, with a particular interest
    in <em>interpretable, physics-informed methods</em> for biomedical systems.
  </div>
</div>

<!-- Stats -->
<div class="fade-up d5">
  <div class="section-label">at a glance</div>
  <div class="stats-row">
    <div class="stat-box" style="animation-delay:0.5s">
      <div class="stat-num">3+</div>
      <div class="stat-label">Active Labs</div>
    </div>
    <div class="stat-box" style="animation-delay:0.6s">
      <div class="stat-num">2</div>
      <div class="stat-label">1st-Author Papers</div>
    </div>
    <div class="stat-box" style="animation-delay:0.7s">
      <div class="stat-num">Top 5</div>
      <div class="stat-label">Cancer Center</div>
    </div>
    <div class="stat-box" style="animation-delay:0.8s">
      <div class="stat-num">'27</div>
      <div class="stat-label">Graduation</div>
    </div>
  </div>
</div>

<!-- Research -->
<div class="fade-up d5">
  <div class="section-label">research</div>
  <div class="research-grid">

    <div class="r-card primary">
      <div class="r-card-top">
        <span class="r-venue">NeurIPS / MICCAI target</span>
        <span class="r-status pub">In progress</span>
      </div>
      <div class="r-title">Symbolic Regression for Tau Propagation in Alzheimer's Disease</div>
      <div class="r-desc">First-author. Mechanistic-neural hybrid models revealing why local symbolic improvements fail to generalize in coupled systems. Ablation study across library size, component family, and frequency axes.</div>
      <div class="r-pi">PI: <em>Prof. Guorong Wu</em> · ACM Lab, UNC</div>
    </div>

    <div class="r-card">
      <div class="r-card-top">
        <span class="r-venue">Bai Lab · UNC</span>
        <span class="r-status active">Active</span>
      </div>
      <div class="r-title">Cardiac Ultrasound Segmentation</div>
      <div class="r-desc">Deep learning segmentation pipeline using U-Net and DenseNet121 architectures for echocardiography analysis.</div>
      <div class="r-pi">Bai Lab · UNC Chapel Hill</div>
    </div>

    <div class="r-card">
      <div class="r-card-top">
        <span class="r-venue">MD Anderson · Houston</span>
        <span class="r-status incoming">Summer 2026</span>
      </div>
      <div class="r-title">Computational Oncology Research</div>
      <div class="r-desc">Research intern in the Dept. of Bioinformatics &amp; Computational Biology. Contributing to frontier cancer research at the #1 ranked oncology center in the US.</div>
      <div class="r-pi">Supervisor: <em>Dr. Tao Wang</em></div>
    </div>

    <div class="r-card">
      <div class="r-card-top">
        <span class="r-venue">UC Irvine</span>
        <span class="r-status incoming">Summer 2026</span>
      </div>
      <div class="r-title">ML for Optical Blood Flow Imaging</div>
      <div class="r-desc">Collaborating with Prof. Bernard Choi on applying machine learning to optical coherence tomography and blood imaging. Target: Q3 2026 publication.</div>
      <div class="r-pi">PI: <em>Prof. Bernard Choi</em> · UC Irvine</div>
    </div>

  </div>
</div>

<!-- Stack -->
<div class="fade-up d6">
  <div class="section-label">technical stack</div>
  <div class="stack-grid">
    <span class="stack-tag hot">PyTorch</span>
    <span class="stack-tag hot">Python</span>
    <span class="stack-tag hot">Symbolic Regression</span>
    <span class="stack-tag">U-Net</span>
    <span class="stack-tag">DenseNet</span>
    <span class="stack-tag">Neural ODEs</span>
    <span class="stack-tag">Next.js</span>
    <span class="stack-tag">React</span>
    <span class="stack-tag">Supabase</span>
    <span class="stack-tag">NumPy</span>
    <span class="stack-tag">scikit-learn</span>
    <span class="stack-tag">Linux / SLURM</span>
    <span class="stack-tag">Multi-GPU Training</span>
    <span class="stack-tag">Java</span>
    <span class="stack-tag">SQL</span>
  </div>
</div>

<!-- Timeline -->
<div class="fade-up d7">
  <div class="section-label">timeline</div>
  <div class="timeline">
    <div class="tl-item">
      <div class="tl-date">2025 – present</div>
      <div class="tl-content">
        <div class="tl-title">First-author researcher · ACM Lab</div>
        <div class="tl-sub">Symbolic regression + Alzheimer's tau propagation · UNC School of Medicine</div>
      </div>
    </div>
    <div class="tl-item">
      <div class="tl-date">2024 – present</div>
      <div class="tl-content">
        <div class="tl-title">Undergraduate Research Assistant · Bai Lab</div>
        <div class="tl-sub">Cardiac ultrasound segmentation · UNC Chapel Hill</div>
      </div>
    </div>
    <div class="tl-item">
      <div class="tl-date">2024 – present</div>
      <div class="tl-content">
        <div class="tl-title">Co-founder · PaperTrail</div>
        <div class="tl-sub">Undergraduate research recruitment platform · Next.js, React, Supabase</div>
      </div>
    </div>
    <div class="tl-item">
      <div class="tl-date">Summer 2026</div>
      <div class="tl-content">
        <div class="tl-title">Research Intern · MD Anderson Cancer Center</div>
        <div class="tl-sub">Dept. of Bioinformatics &amp; Computational Biology · Supervised by Dr. Tao Wang</div>
      </div>
    </div>
    <div class="tl-item">
      <div class="tl-date">Summer 2026</div>
      <div class="tl-content">
        <div class="tl-title">Research Collaborator · UC Irvine</div>
        <div class="tl-sub">ML for optical blood flow imaging · Prof. Bernard Choi</div>
      </div>
    </div>
    <div class="tl-item">
      <div class="tl-date">Dec 2027</div>
      <div class="tl-content">
        <div class="tl-title">B.S. Computer Science &amp; Data Science</div>
        <div class="tl-sub">University of North Carolina at Chapel Hill · Early graduation</div>
      </div>
    </div>
  </div>
</div>

<!-- Footer -->
<div class="fade-up d8">
  <footer class="footer">
    <div class="footer-links">
      <a class="footer-link" href="mailto:harrison@unc.edu">email ↗</a>
      <a class="footer-link" href="#">linkedin ↗</a>
      <a class="footer-link" href="#">arxiv ↗</a>
      <a class="footer-link" href="#">papertrail ↗</a>
    </div>
    <div class="footer-note">UNC Chapel Hill · Chapel Hill, NC</div>
  </footer>
</div>

</body>
</html>
