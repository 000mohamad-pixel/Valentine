<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>POV: Flowers - Mock</title>
  <style>
    :root{
      --bg: #0b0b0f;
      --card: rgba(255,255,255,0.08);
      --text: #fff;
      --muted: rgba(255,255,255,0.75);
      --accent: #ff76a6;
    }
    * { box-sizing: border-box; }
    html, body { height: 100%; margin: 0; background: var(--bg); color: var(--text); font-family: Inter, system-ui, -apple-system, Arial; }
    .scene {
      position: relative;
      height: 100vh;
      width: 100%;
      overflow: hidden;
      display: grid;
      place-items: center;
    }
    /* SVG garden background (stylized) */
    .garden {
      position: absolute;
      bottom: -5%;
      width: 130%;
      height: 60%;
      left: 50%;
      transform: translateX(-50%);
      opacity: 0.95;
      filter: saturate(1.2);
      pointer-events: none;
    }
    /* Overlay caption (POV text) */
    .caption {
      position: absolute;
      top: 50px;
      left: 50%;
      transform: translateX(-50%);
      text-align: center;
      padding: 8px 14px;
      border-radius: 999px;
      background: rgba(0,0,0,0.28);
      border: 1px solid rgba(255,255,255,0.15);
      font-weight: 600;
      font-size: 1.05rem;
      letter-spacing: .2px;
      color: #fff;
      backdrop-filter: blur(2px);
      z-index: 3;
    }
    /* Floating particles (flowers) */
    .particle {
      position: absolute;
      width: 28px;
      height: 28px;
      pointer-events: none;
      animation: float 8s linear infinite;
      opacity: 0;
    }
    @keyframes float {
      0% { transform: translateY(20px) translateX(0) scale(0.9); opacity: 0; }
      10% { opacity: 1; }
      100% { transform: translateY(-120vh) translateX(var(--dx)) scale(0.9); opacity: 0; }
    }
    /* Mock RHS UI (reactions) */
    .side {
      position: absolute;
      right: 10px;
      bottom: 140px;
      width: 120px;
      display: grid;
      gap: 14px;
      z-index: 2;
    }
    .icon {
      width: 46px; height: 46px;
      border-radius: 12px;
      background: rgba(255,255,255,0.08);
      display: grid; place-items: center;
      border: 1px solid rgba(255,255,255,0.15);
      cursor: default;
    }
    .icon span { font-size: 20px; }
    /* Mock profile bar at bottom-left */
    .footer {
      position: absolute;
      left: 12px;
      bottom: 12px;
      display: flex; align-items: center; gap: 10px;
      padding: 8px 12px;
      border-radius: 999px;
      background: rgba(0,0,0,0.38);
      border: 1px solid rgba(255,255,255,0.15);
      z-index: 2;
    }
    .avatar {
      width: 28px; height: 28px; border-radius: 50%;
      background: #ddd; display: inline-block;
    }
    .name { font-weight: 600; font-size: 0.95rem; }
    .comment {
      width: min(90vw, 720px);
      position: absolute;
      bottom: 20px;
      left: 50%;
      transform: translateX(-50%);
      display: flex;
      align-items: center;
      gap: 8px;
      padding: 10px 12px;
      border-radius: 14px;
      background: rgba(0,0,0,0.42);
      border: 1px solid rgba(255,255,255,0.15);
      z-index: 2;
    }
    .input {
      flex: 1;
      background: rgba(255,255,255,0.12);
      border: 1px solid rgba(255,255,255,0.25);
      border-radius: 999px;
      padding: 10px 14px;
      color: #fff;
      outline: none;
    }
    .button {
      padding: 10px 14px;
      border-radius: 999px;
      border: 1px solid rgba(255,255,255,0.25);
      background: linear-gradient(135deg, rgba(255,255,255,0.15), rgba(255,255,255,0.05));
      color: #fff;
      cursor: pointer;
    }
    /* Responsive tweaks */
    @media (max-width: 700px) {
      .side { display: none; }
      .caption { font-size: 0.95rem; padding: 8px 12px; }
      .comment { bottom: 14px; left: 50%; transform: translateX(-50%); }
    }
  </style>
</head>
<body>
  <div class="scene" aria-label="POV flower scene">
    <!-- Caption -->
    <div class="caption" id="caption">POV: she wanted flowers so I coded them</div>

    <!-- Garden SVG (stylized) -->
    <svg class="garden" viewBox="0 0 1200 600" aria-label="Garden background" preserveAspectRatio="xMidYMid slice">
      <defs>
        <linearGradient id="g1" x1="0" y1="0" x2="0" y2="1">
          <stop stop-color="#1b5e20" offset="0"/>
          <stop stop-color="#064b22" offset="1"/>
        </linearGradient>
      </defs>
      <rect width="1200" height="600" fill="#0a0a0f"/>
      <!-- simple hill gradient -->
      <ellipse cx="600" cy="520" rx="600" ry="120" fill="url(#g1)"/>
      <!-- flowers (stylized) -->
      <g fill="#ff7ac3" opacity="0.9">
        <circle cx="210" cy="420" r="6"/><circle cx="220" cy="410" r="6"/><circle cx="215" cy="435" r="6"/>
        <circle cx="260" cy="430" r="6"/><circle cx="270" cy="420" r="6"/>
      </g>
      <!-- a few blades -->
      <path d="M100 520 C120 450, 150 470, 170 520" stroke="#4cc9f0" stroke-width="6" fill="none" />
      <path d="M340 520 C360 460, 400 480, 420 520" stroke="#8bd36a" stroke-width="6" fill="none" />
      <!-- simple flowers along the bottom -->
      <g fill="#ff6b9a" opacity="0.95">
        <circle cx="480" cy="510" r="7"/>
        <circle cx="520" cy="520" r="7"/>
        <circle cx="560" cy="510" r="7"/>
      </g>
    </svg>

    <!-- Mock reaction icons (right side) -->
    <div class="side" aria-label="Reactions">
      <div class="icon" title="Like"><span>❤️</span></div>
      <div class="icon" title="Comment"><span>💬</span></div>
      <div class="icon" title="Share"><span>🔗</span></div>
    </div>

    <!-- Mock footer with avatar and name -->
    <div class="footer" aria-label="Author">
      <span class="avatar" aria-label="avatar"></span>
      <span class="name">bluduven8</span>
      <span style="opacity:.8;">• 1h</span>
    </div>

    <!-- Floating flower particles (generated by JS) -->
  </div>

  <!-- Mock comment input -->
  <div class="comment" aria-label="Comment input">
    <input class="input" id="commentInput" placeholder="Kommentar hinzufügen ..." />
    <button class="button" id="sendBtn">Posten</button>
  </div>

  <script>
    // Simple particle generator to mimic floating flowers
    const scene = document.querySelector('.scene');
    const caption = document.getElementById('caption');

    function spawnParticle() {
      const p = document.createElement('div');
      p.className = 'particle';
      // random flower emoji or SVG-like circle
      const symbols = ['🌸','💮','🌼','🌺','🌷'];
      const s = symbols[Math.floor(Math.random()*symbols.length)];
      p.style.left = (Math.random() * 100) + 'vw';
      p.style.bottom = (Math.random()*40 + 5) + 'vh';
      p.style.fontSize = (20 + Math.random()*18) + 'px';
      p.style.lineHeight = '1';
      p.style.width = '28px';
      p.style.height = '28px';
      // use emoji as content
      p.innerText = s;
      // slight horizontal drift
      const dx = (Math.random() * 40 - 20) + 'px';
      p.style.setProperty('--dx', dx);
      // random delay
      p.style.animationDelay = (Math.random()*2).toFixed(2) + 's';
      // Add and remove after animation
      scene.appendChild(p);
      // Remove after 8s
      setTimeout(() => p.remove(), 8000);
    }

    // spawn every 700-1200ms
    setInterval(spawnParticle, 900);

    // Comment posting logic (demo)
    const input = document.getElementById('commentInput');
    const btn = document.getElementById('sendBtn');
    btn.addEventListener('click', () => {
      const text = input.value.trim();
      if (!text) return;
      // Simple mock: prepend to caption as a tiny toast-like line
      const note = document.createElement('div');
      note.style.position = 'absolute';
      note.style.top = '60px';
      note.style.left = '50%';
      note.style.transform = 'translateX(-50%)';
      note.style.padding = '6px 12px';
      note.style.borderRadius = '999px';
      note.style.background = 'rgba(0,0,0,0.5)';
      note.style.border = '1px solid rgba(255,255,255,0.25)';
      note.style.color = '#fff';
      note.style.fontSize = '0.85rem';
      note.style.zIndex = '4';
      note.textContent = 'You: ' + text;
      document.body.appendChild(note);
      setTimeout(() => note.remove(), 2200);
      input.value = '';
    });

    // Accessibility: allow Enter to post
    input.addEventListener('keydown', (e) => {
      if (e.key === 'Enter') btn.click();
    });
  </script>
</body>
</html>
