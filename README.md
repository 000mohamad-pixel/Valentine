<!doctype html>
<html lang="de">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Willst du mein Valentine sein?</title>

  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;500;700&display=swap" rel="stylesheet">

  <style>
    :root{
      --bg-1: #0a0e17;
      --bg-2: #1a1f2e;
      --accent-1: #ff3366;
      --accent-2: #ff6699;
      --muted: #a0a0b0;
      --highlight: #ff4081;
      --text-light: #ffffff;
      --text-dark: #f0f0f0;
    }

    * { box-sizing: border-box; }
    html,body{height:100%;margin:0;font-family:"Poppins",system-ui,-apple-system,Segoe UI,Roboto,"Helvetica Neue",Arial;}
    body{
      display:flex;
      align-items:center;
      justify-content:center;
      padding:28px;
      background: var(--bg-1);
      -webkit-font-smoothing:antialiased;
      overflow:hidden;
      position: relative;
    }

    /* Sternenhimmel */
    .stars {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      pointer-events: none;
      z-index: 1;
    }
    
    .star {
      position: absolute;
      background: white;
      border-radius: 50%;
      animation: twinkle 4s infinite;
    }

    /* card */
    .card{
      width:min(980px,96vw);
      display:grid;
      grid-template-columns: 1fr 380px;
      gap:22px;
      background: rgba(26, 31, 46, 0.85);
      border-radius:18px;
      padding:28px;
      box-shadow: 0 28px 80px rgba(255, 64, 129, 0.15);
      position:relative;
      overflow:visible;
      border: 1px solid rgba(255, 64, 129, 0.1);
      backdrop-filter: blur(10px);
      z-index: 10;
    }
    @media (max-width:900px){ .card{ grid-template-columns:1fr; } }

    h1{ margin:0 0 8px 0; font-size:clamp(20px,3.2vw,34px); color:var(--text-light); line-height:1.02; }
    h1 .name { 
      color: var(--highlight); 
      font-weight:800;
      text-shadow: 0 0 20px rgba(255, 64, 129, 0.5);
    }

    .lead{ 
      margin:8px 0 20px 0; 
      color:var(--muted); 
      font-size:16px; 
      line-height:1.45; 
    }

    .actions{ 
      display:flex; 
      gap:12px; 
      align-items:center; 
      flex-wrap:wrap; 
      position:relative; 
      z-index:12; 
    }
    
    .btn{
      cursor:pointer;
      border:0;
      padding:12px 18px;
      border-radius:999px;
      font-weight:700;
      font-size:16px;
      box-shadow:0 8px 26px rgba(0,0,0,0.15); 
      transition: all 0.3s ease;
      user-select:none; 
      background: linear-gradient(90deg,var(--accent-1),var(--accent-2)); 
      color:#fff;
      position: relative;
      overflow: hidden;
    }
    
    .btn::before {
      content: '';
      position: absolute;
      top: 0;
      left: -100%;
      width: 100%;
      height: 100%;
      background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
      transition: left 0.7s;
    }
    
    .btn:hover::before {
      left: 100%;
    }

    /* YES button */
    .yes {
      --scale: 1;
      transform-origin: center;
      transform: scale(var(--scale));
      box-shadow: 0 10px 30px rgba(255, 51, 102, 0.3);
    }

    /* NO button - BEWEGT SICH LEICHT */
    .no {
      background: rgba(255, 255, 255, 0.1);
      color: var(--text-light);
      border: 1px solid rgba(255, 64, 129, 0.3);
      padding:10px 16px;
      border-radius:12px;
      min-width:100px;
      box-shadow: 0 8px 24px rgba(0,0,0,0.1);
      font-weight:700;
      transition: all 0.3s ease;
      opacity: 0.85;
      cursor: pointer;
      position: relative;
      z-index: 20;
      /* Startposition: neben dem Ja-Button */
      margin-left: 0;
    }
    
    .no:hover{
      opacity:1;
      background: rgba(255, 64, 129, 0.2);
      transform: translateY(-2px);
    }

    /* visual area & SVG */
    .visual{ 
      display:flex; 
      align-items:center; 
      justify-content:center; 
      pointer-events:none; 
    }
    
    .scene{
      width:340px;
      height:340px;
      border-radius:20px;
      background: rgba(255, 64, 129, 0.05);
      display:grid;
      place-items:center;
      position:relative;
      overflow:hidden;
      border: 1px solid rgba(255, 64, 129, 0.1);
    }
    
    .cat-svg{ 
      width:260px; 
      height:260px; 
      pointer-events:none; 
      display:block; 
      transform-origin:center; 
      filter: drop-shadow(0 0 20px rgba(255, 64, 129, 0.3));
    }

    /* heart pulse */
    @keyframes heartPulse { 
      0%{transform:scale(1);}
      50%{transform:scale(1.12);}
      100%{transform:scale(1);} 
    }

    /* floating bg hearts */
    .bg-heart { 
      position:absolute; 
      font-size:18px; 
      opacity:0; 
      pointer-events:none; 
      z-index: 5;
    }

    /* modal */
    .overlay{ 
      position:fixed; 
      inset:0; 
      display:grid; 
      place-items:center; 
      z-index:1500; 
      background: rgba(0, 0, 0, 0.7);
      opacity:0; 
      pointer-events:none; 
      transition: opacity 220ms; 
      backdrop-filter: blur(4px); 
    }
    
    .overlay.show{ opacity:1; pointer-events:auto; }
    
    .modal{ 
      background: rgba(26, 31, 46, 0.95);
      border-radius:14px;
      padding:22px 24px;
      width:min(520px,94%);
      text-align:center; 
      box-shadow:0 22px 64px rgba(0,0,0,0.25);
      border: 1px solid rgba(255, 64, 129, 0.2);
      color: var(--text-light);
    }

    /* Animationen */
    @keyframes twinkle {
      0%, 100% {
        opacity: 0.3;
        transform: scale(1);
      }
      50% {
        opacity: 0.8;
        transform: scale(1.1);
      }
    }

    @keyframes floatUpFade { 
      0% { 
        transform: translateY(0) rotate(0deg) scale(1); 
        opacity: 0; 
      } 
      20% { 
        opacity: 0.8; 
      } 
      100% { 
        transform: translateY(-150px) rotate(180deg) scale(0.5); 
        opacity: 0; 
      } 
    }

    @keyframes gentleBeat {
      0%, 100% {
        transform: scale(1);
      }
      50% {
        transform: scale(1.08);
      }
    }

    /* Wackel-Animation für NO Button */
    @keyframes wiggle {
      0% { transform: translateX(0) rotate(0deg); }
      25% { transform: translateX(-10px) rotate(-2deg); }
      50% { transform: translateX(10px) rotate(2deg); }
      75% { transform: translateX(-5px) rotate(-1deg); }
      100% { transform: translateX(0) rotate(0deg); }
    }

    /* Herzen Partikel */
    .heart-particle {
      position: fixed;
      font-size: 20px;
      color: #ff6699;
      opacity: 0;
      pointer-events: none;
      z-index: 100;
      animation: floatUpFade 1.5s ease-in forwards;
    }

    /* respects reduced motion */
    @media (prefers-reduced-motion: reduce){
      .no, .btn, .cat-svg * , .bg-heart, .heart-particle { 
        transition:none!important; 
        animation:none!important; 
      }
    }
  </style>
</head>
<body>
  <!-- Sternenhimmel -->
  <div class="stars" id="stars"></div>

  <main class="card" aria-labelledby="main-title">
    <section class="left">
      <h1 id="main-title">Willst du mein <span class="name">Valentine</span> sein?</h1>

      <p class="lead">
        Unter diesem funkelnden Sternenhimmel frage ich dich:
        Möchtest du mit mir den Valentinstag verbringen?
      </p>

      <div class="actions" id="actions">
        <button class="btn yes" id="yesBtn" aria-haspopup="dialog">💖 Ja, sehr gern!</button>
        <button class="no" id="noBtn" aria-label="Nein">Nein</button>
      </div>
    </section>

    <aside class="visual" aria-hidden="true">
      <div class="scene" id="scene">
        <!-- animated SVG cat hugging a heart -->
        <svg class="cat-svg" viewBox="0 0 300 300" xmlns="http://www.w3.org/2000/svg" role="img" aria-hidden="true">
          <defs>
            <linearGradient id="heartGrad" x1="0" x2="1">
              <stop offset="0" stop-color="#ff3366"/>
              <stop offset="1" stop-color="#ff6699"/>
            </linearGradient>
            <filter id="soft" x="-50%" y="-50%" width="200%" height="200%">
              <feDropShadow dx="0" dy="10" stdDeviation="14" flood-opacity="0.20" flood-color="#ff3366"/>
            </filter>
            <radialGradient id="starGlow" cx="50%" cy="50%" r="50%">
              <stop offset="0%" stop-color="#ffffff" stop-opacity="0.8"/>
              <stop offset="100%" stop-color="#ffffff" stop-opacity="0"/>
            </radialGradient>
          </defs>

          <!-- Hintergrundkreis mit Sternenglanz -->
          <circle cx="150" cy="150" r="110" fill="url(#starGlow)" opacity="0.1"/>

          <!-- body -->
          <g transform="translate(0,12)">
            <ellipse cx="150" cy="190" rx="95" ry="70" fill="#2a1f2e"/>
            <ellipse cx="150" cy="182" rx="78" ry="54" fill="#3a2f3e"/>
          </g>

          <!-- head -->
          <g id="head" transform="translate(0,-6)">
            <circle cx="150" cy="98" r="60" fill="#3a2f3e"/>
            <!-- ears -->
            <path d="M108 54 L94 28 L126 44 Z" fill="#3a2f3e"/>
            <path d="M192 54 L206 28 L174 44 Z" fill="#3a2f3e"/>
            <path d="M108 54 L100 38 L120 48 Z" fill="#4a3f4e"/>
            <path d="M192 54 L200 38 L180 48 Z" fill="#4a3f4e"/>
            <!-- eyes -->
            <ellipse cx="132" cy="100" rx="8" ry="10" fill="#ffccff"/>
            <ellipse cx="168" cy="100" rx="8" ry="10" fill="#ffccff"/>
            <!-- cheeks -->
            <circle cx="118" cy="112" r="5" fill="#ff6699" opacity="0.7"/>
            <circle cx="182" cy="112" r="5" fill="#ff6699" opacity="0.7"/>
            <!-- mouth -->
            <path d="M142 124 q6 8 16 0" stroke="#ffccff" stroke-width="2" stroke-linecap="round" fill="none"/>
          </g>

          <!-- left arm -->
          <g id="leftArm" transform="translate(0,0)" style="transform-origin:72px 170px;">
            <path d="M80 160 q40 12 40 40 q-10 18 -34 14 q-12 -6 -10 -36 z" fill="#3a2f3e" stroke="#4a3f4e" stroke-width="2"/>
            <ellipse cx="100" cy="186" rx="12" ry="10" fill="#3a2f3e" stroke="#4a3f4e" stroke-width="1.5"/>
          </g>

          <!-- right arm -->
          <g id="rightArm" transform="translate(0,0)" style="transform-origin:228px 170px;">
            <path d="M220 160 q-40 12 -40 40 q10 18 34 14 q12 -6 10 -36 z" fill="#3a2f3e" stroke="#4a3f4e" stroke-width="2"/>
            <ellipse cx="200" cy="186" rx="12" ry="10" fill="#3a2f4e" stroke="#4a3f4e" stroke-width="1.5"/>
          </g>

          <!-- heart -->
          <g id="heartGroup" filter="url(#soft)">
            <g transform="translate(150,160)">
              <path id="heart" d="M0 -18 C -18 -40, -48 -28, -48 -4 C -48 16, -24 30, 0 44 C 24 30, 48 16, 48 -4 C 48 -28, 18 -40, 0 -18 Z"
                    fill="url(#heartGrad)" transform="scale(0.45) translate(0,10)"/>
            </g>
          </g>
        </svg>
      </div>
    </aside>
  </main>

  <div class="overlay" id="overlay" role="dialog" aria-modal="true" aria-hidden="true">
    <div class="modal" role="document">
      <h2>Juhuu — Danke! 💕</h2>
      <p>Du hast mir gerade einen großartigen Moment unter den Sternen geschenkt. Ich freue mich sehr!</p>
      <div style="display:flex;gap:10px;justify-content:center;margin-top:12px;">
        <button class="btn" id="closeModal">Schließen</button>
      </div>
    </div>
  </div>

  <script>
    /* Sternenhimmel erstellen */
    (function createStars() {
      const starsContainer = document.getElementById('stars');
      if (!starsContainer) return;
      
      for (let i = 0; i < 150; i++) {
        const star = document.createElement('div');
        star.classList.add('star');
        
        const size = Math.random() * 2 + 1;
        star.style.width = `${size}px`;
        star.style.height = `${size}px`;
        star.style.left = `${Math.random() * 100}%`;
        star.style.top = `${Math.random() * 100}%`;
        star.style.animationDelay = `${Math.random() * 4}s`;
        star.style.animationDuration = `${3 + Math.random() * 5}s`;
        
        starsContainer.appendChild(star);
      }
    })();

    /* Sanfte Herzen Partikel */
    function createHeartParticle(x, y) {
      const heart = document.createElement('div');
      heart.classList.add('heart-particle');
      heart.innerHTML = '❤️';
      heart.style.left = `${x}px`;
      heart.style.top = `${y}px`;
      heart.style.color = ['#ff3366', '#ff6699', '#ff9966'][Math.floor(Math.random() * 3)];
      heart.style.fontSize = `${10 + Math.random() * 15}px`;
      heart.style.animationDuration = `${1 + Math.random()}s`;
      
      document.body.appendChild(heart);
      
      setTimeout(() => {
        if (heart.parentNode) heart.remove();
      }, 1500);
    }

    /* Arm and heart animations */
    (function animateCat() {
      const leftArm = document.getElementById('leftArm');
      const rightArm = document.getElementById('rightArm');
      const heart = document.getElementById('heart');

      if (leftArm.animate && rightArm.animate) {
        // Sanfte Armbewegung
        leftArm.animate([
          { transform: 'rotate(0deg) translateY(0)' },
          { transform: 'rotate(-15deg) translateY(-4px)' },
          { transform: 'rotate(0deg) translateY(0)' }
        ], { duration: 2000, iterations: Infinity, easing: 'ease-in-out' });

        rightArm.animate([
          { transform: 'rotate(0deg) translateY(0)' },
          { transform: 'rotate(15deg) translateY(-4px)' },
          { transform: 'rotate(0deg) translateY(0)' }
        ], { duration: 2000, iterations: Infinity, easing: 'ease-in-out' });
      }

      // Herz-Puls
      if (heart && heart.animate) {
        heart.animate([
          { transform: 'scale(1)', offset: 0 },
          { transform: 'scale(1.15)', offset: 0.5 },
          { transform: 'scale(1)', offset: 1 }
        ], { duration: 1200, iterations: Infinity, easing: 'ease-in-out' });
      } else if (heart) {
        heart.style.animation = 'gentleBeat 1200ms infinite ease-in-out';
      }
    })();

    /* YES button animation */
    function animateYesGrow(yesBtn, scale) {
      yesBtn.style.setProperty('--scale', scale.toFixed(3));
      
      if (yesBtn.animate) {
        // Sanfte Glow-Animation
        yesBtn.animate([
          { boxShadow: '0 10px 30px rgba(255, 51, 102, 0.3)' },
          { boxShadow: '0 15px 40px rgba(255, 51, 102, 0.5)' },
          { boxShadow: '0 10px 30px rgba(255, 51, 102, 0.3)' }
        ], { duration: 600, easing: 'ease-out' });
        
        // Sanfte Scale-Animation
        yesBtn.animate([
          { transform: `scale(${scale * 0.98})` },
          { transform: `scale(${scale * 1.03})` },
          { transform: `scale(${scale})` }
        ], { duration: 500, easing: 'cubic-bezier(.2,1.0,.36,1.0)' });
      }
    }

    /* NO button behavior - JETZT BEWEGT ER SICH WIRKLICH */
    (function noButtonBehavior() {
      const noBtn = document.getElementById('noBtn');
      const yesBtn = document.getElementById('yesBtn');

      let yesScale = 1;
      const maxYesScale = 1.8;
      const growStep = 0.08;
      let clickCount = 0;
      
      // Funktion für Wackel-Animation
      function wiggleNoButton() {
        clickCount++;
        
        // 1. Animation starten
        noBtn.style.animation = 'wiggle 0.5s ease-in-out';
        
        // 2. Button kurz vergrößern
        noBtn.style.transform = 'scale(1.1)';
        noBtn.style.boxShadow = '0 12px 30px rgba(255, 64, 129, 0.2)';
        
        // 3. Nach Animation zurücksetzen
        setTimeout(() => {
          noBtn.style.animation = '';
          noBtn.style.transform = 'scale(1)';
          noBtn.style.boxShadow = '0 8px 24px rgba(0,0,0,0.1)';
        }, 500);
        
        // "Ja"-Button wächst
        increaseYes();
        
        // Kleine Herzen erscheinen
        createHeartParticles();
      }
      
      function increaseYes() {
        yesScale = Math.min(maxYesScale, +(yesScale + growStep).toFixed(3));
        animateYesGrow(yesBtn, yesScale);
      }
      
      function createHeartParticles() {
        const rect = noBtn.getBoundingClientRect();
        // 2-3 kleine Herzen
        const count = 2 + Math.floor(Math.random() * 2);
        for (let i = 0; i < count; i++) {
          setTimeout(() => {
            createHeartParticle(
              rect.left + rect.width / 2 + (Math.random() - 0.5) * 40,
              rect.top + rect.height / 2 + (Math.random() - 0.5) * 20
            );
          }, i * 100);
        }
      }

      // Klick-Event
      noBtn.addEventListener('click', function(e) {
        e.stopPropagation();
        wiggleNoButton();
        
        // Text ändert sich kurz
        const responses = [
          "Hmm...", 
          "Sicher?", 
          "Wirklich?", 
          "Echt?", 
          "Bist du dir sicher?",
          "Vielleicht?",
          "Überleg nochmal!",
          "Neeein!",
          "Bestimmt nicht!"
        ];
        
        const responseIndex = Math.min(clickCount - 1, responses.length - 1);
        this.textContent = responses[responseIndex] || "Auf keinen Fall!";
        
        // Nach kurzer Zeit zurück zu "Nein"
        setTimeout(() => {
          this.textContent = "Nein";
        }, 1000);
      });

      // Touch für Mobilgeräte
      noBtn.addEventListener('touchstart', function(e) {
        e.preventDefault();
        wiggleNoButton();
        this.textContent = "Nope!";
        setTimeout(() => {
          this.textContent = "Nein";
        }, 1000);
      });

      // Verhindere Event-Bubbling
      noBtn.addEventListener('mousedown', function(e) {
        e.stopPropagation();
      });
      
      // Hover-Effekt
      noBtn.addEventListener('mouseenter', function() {
        this.style.opacity = '1';
        this.style.transform = 'translateY(-2px)';
      });
      
      noBtn.addEventListener('mouseleave', function() {
        this.style.opacity = '0.85';
        this.style.transform = 'translateY(0)';
      });
    })();

    /* YES behavior: modal */
    (function yesBehavior(){
      const yesBtn = document.getElementById('yesBtn');
      const overlay = document.getElementById('overlay');
      const closeModal = document.getElementById('closeModal');

      function createCelebrationHearts() {
        for (let i = 0; i < 20; i++) {
          setTimeout(() => {
            createHeartParticle(
              Math.random() * window.innerWidth,
              window.innerHeight + 50
            );
          }, i * 80);
        }
      }

      yesBtn.addEventListener('click', () => {
        overlay.classList.add('show');
        overlay.setAttribute('aria-hidden','false');
        createCelebrationHearts();
        
        // Sanfter Button-Effekt
        if (yesBtn.animate) {
          yesBtn.animate([
            { transform: 'scale(1.05)' },
            { transform: 'scale(1)' }
          ], { duration: 400, easing: 'cubic-bezier(.2,1,.36,1)' });
        }
        
        const close = document.getElementById('closeModal');
        if (close) close.focus();
      });

      const close = document.getElementById('closeModal');
      if (close) close.addEventListener('click', () => {
        overlay.classList.remove('show');
        overlay.setAttribute('aria-hidden','true');
        yesBtn.focus();
      });

      window.addEventListener('keydown', (e) => {
        if (e.key === 'Escape') {
          overlay.classList.remove('show');
          overlay.setAttribute('aria-hidden','true');
        }
      });
    })();

    /* Sanfte automatische Herzen im Hintergrund */
    (function createBackgroundHearts() {
      setInterval(() => {
        if (Math.random() > 0.8) {
          createHeartParticle(
            Math.random() * window.innerWidth,
            window.innerHeight + 20
          );
        }
      }, 3000);
    })();
  </script>
</body>
</html>
