<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>You Rock!</title>
  <link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Permanent+Marker&display=swap" rel="stylesheet"/>
  <style>
    :root {
      --bg: #0a0a0a;
      --yellow: #f5e642;
      --orange: #ff6b35;
      --white: #f0ece2;
      --red: #e63946;
    }

    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      background: var(--bg);
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      overflow: hidden;
      font-family: 'Bebas Neue', sans-serif;
      cursor: none;
    }

    /* Custom cursor */
    .cursor {
      position: fixed;
      width: 20px; height: 20px;
      background: var(--yellow);
      border-radius: 50%;
      pointer-events: none;
      transform: translate(-50%, -50%);
      transition: transform 0.1s ease, width 0.2s, height 0.2s;
      z-index: 9999;
      mix-blend-mode: difference;
    }

    /* Noise grain overlay */
    body::before {
      content: '';
      position: fixed; inset: 0;
      background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.85' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.07'/%3E%3C/svg%3E");
      pointer-events: none;
      z-index: 100;
      opacity: 0.4;
    }

    /* Radial glow bg */
    .glow-bg {
      position: fixed; inset: 0;
      background: radial-gradient(ellipse 60% 50% at 50% 50%, #1a1200 0%, var(--bg) 70%);
      z-index: 0;
    }

    .scene {
      position: relative;
      z-index: 10;
      text-align: center;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 0;
    }

    /* Stars / sparks */
    .sparks {
      position: fixed; inset: 0;
      pointer-events: none;
      z-index: 5;
    }
    .spark {
      position: absolute;
      width: 4px; height: 4px;
      background: var(--yellow);
      border-radius: 50%;
      opacity: 0;
      animation: sparkle var(--dur, 3s) var(--delay, 0s) infinite ease-in-out;
    }
    @keyframes sparkle {
      0%   { opacity: 0; transform: scale(0) translateY(0); }
      30%  { opacity: 1; transform: scale(1) translateY(-10px); }
      70%  { opacity: 0.5; }
      100% { opacity: 0; transform: scale(0) translateY(-40px); }
    }

    /* YOU */
    .you {
      font-family: 'Permanent Marker', cursive;
      font-size: clamp(3rem, 10vw, 7rem);
      color: var(--white);
      letter-spacing: 0.05em;
      line-height: 1;
      opacity: 0;
      transform: translateY(-40px) rotate(-3deg);
      animation: dropIn 0.7s 0.2s cubic-bezier(0.175,0.885,0.32,1.275) forwards;
    }

    /* ROCK */
    .rock {
      font-family: 'Bebas Neue', sans-serif;
      font-size: clamp(7rem, 28vw, 20rem);
      line-height: 0.85;
      letter-spacing: -0.02em;
      color: transparent;
      -webkit-text-stroke: 3px var(--yellow);
      position: relative;
      opacity: 0;
      transform: scale(0.6) rotate(2deg);
      animation: explodeIn 0.8s 0.5s cubic-bezier(0.175,0.885,0.32,1.275) forwards;
    }
    .rock::after {
      content: 'ROCK';
      position: absolute; inset: 0;
      color: var(--yellow);
      -webkit-text-stroke: 0;
      clip-path: polygon(0 60%, 100% 60%, 100% 100%, 0 100%);
      animation: fillPulse 2s 1.5s ease-in-out infinite alternate;
    }
    @keyframes fillPulse {
      from { opacity: 0.15; }
      to   { opacity: 0.5; }
    }

    /* EXCLAMATION */
    .bang {
      font-family: 'Bebas Neue', sans-serif;
      font-size: clamp(5rem, 18vw, 13rem);
      color: var(--orange);
      line-height: 0.9;
      display: inline-block;
      opacity: 0;
      animation: bangIn 0.5s 1.1s cubic-bezier(0.175,0.885,0.32,1.275) forwards,
                 bangBounce 0.4s 1.7s ease-in-out 3;
    }

    /* Subtitle */
    .sub {
      font-family: 'Permanent Marker', cursive;
      font-size: clamp(1rem, 3vw, 1.6rem);
      color: var(--orange);
      letter-spacing: 0.3em;
      text-transform: uppercase;
      opacity: 0;
      animation: fadeUp 0.6s 1.5s ease forwards;
      margin-top: 0.5rem;
    }

    /* Underline decoration */
    .underline-deco {
      width: 0;
      height: 4px;
      background: linear-gradient(90deg, var(--red), var(--yellow), var(--orange));
      border-radius: 2px;
      margin-top: 0.8rem;
      animation: growLine 0.8s 1.8s ease forwards;
    }
    @keyframes growLine {
      from { width: 0; }
      to   { width: min(60vw, 500px); }
    }

    @keyframes dropIn {
      to { opacity: 1; transform: translateY(0) rotate(-3deg); }
    }
    @keyframes explodeIn {
      to { opacity: 1; transform: scale(1) rotate(0deg); }
    }
    @keyframes bangIn {
      to { opacity: 1; transform: scale(1); }
    }
    @keyframes bangBounce {
      0%   { transform: scale(1); }
      50%  { transform: scale(1.3) rotate(-5deg); }
      100% { transform: scale(1); }
    }
    @keyframes fadeUp {
      from { opacity: 0; transform: translateY(12px); }
      to   { opacity: 1; transform: translateY(0); }
    }

    /* Lightning bolts */
    .bolt {
      position: fixed;
      font-size: clamp(2rem, 5vw, 4rem);
      opacity: 0;
      pointer-events: none;
      animation: boltFlash var(--dur, 4s) var(--delay, 0s) infinite ease;
      z-index: 6;
      filter: drop-shadow(0 0 10px var(--yellow));
    }
    @keyframes boltFlash {
      0%, 90%, 100% { opacity: 0; transform: scale(0.8) rotate(var(--rot,0deg)); }
      92%, 96%      { opacity: 1; transform: scale(1.2) rotate(var(--rot,0deg)); }
      94%            { opacity: 0.5; }
    }
  </style>
</head>
<body>

  <div class="cursor" id="cursor"></div>
  <div class="glow-bg"></div>

  <!-- Sparks -->
  <div class="sparks" id="sparks"></div>

  <!-- Lightning bolts -->
  <span class="bolt" style="top:10%;left:8%;--dur:3.2s;--delay:0.5s;--rot:-15deg">⚡</span>
  <span class="bolt" style="top:15%;right:6%;--dur:4s;--delay:1.2s;--rot:10deg">⚡</span>
  <span class="bolt" style="bottom:18%;left:5%;--dur:5s;--delay:2s;--rot:20deg">🎸</span>
  <span class="bolt" style="bottom:12%;right:8%;--dur:3.7s;--delay:0.8s;--rot:-8deg">⚡</span>
  <span class="bolt" style="top:40%;left:2%;--dur:4.5s;--delay:1.8s;--rot:5deg">🤘</span>
  <span class="bolt" style="top:35%;right:3%;--dur:3.9s;--delay:0.3s;--rot:-12deg">🤘</span>

  <div class="scene">
    <div class="you">you</div>
    <div class="rock">ROCK<span class="bang">!</span></div>
    <div class="sub">never stop being awesome</div>
    <div class="underline-deco"></div>
  </div>

  <script>
    // Custom cursor
    const cursor = document.getElementById('cursor');
    document.addEventListener('mousemove', e => {
      cursor.style.left = e.clientX + 'px';
      cursor.style.top  = e.clientY + 'px';
    });

    // Generate sparks
    const sparksEl = document.getElementById('sparks');
    for (let i = 0; i < 40; i++) {
      const s = document.createElement('div');
      s.className = 'spark';
      s.style.cssText = `
        left: ${Math.random()*100}%;
        top: ${Math.random()*100}%;
        --dur: ${2 + Math.random()*4}s;
        --delay: ${Math.random()*5}s;
        width: ${2+Math.random()*4}px;
        height: ${2+Math.random()*4}px;
        background: ${Math.random()>0.5 ? '#f5e642' : '#ff6b35'};
      `;
      sparksEl.appendChild(s);
    }

    // Click burst
    document.addEventListener('click', e => {
      for (let i = 0; i < 8; i++) {
        const b = document.createElement('div');
        b.style.cssText = `
          position:fixed; left:${e.clientX}px; top:${e.clientY}px;
          width:8px; height:8px; border-radius:50%;
          background:${['#f5e642','#ff6b35','#e63946'][Math.floor(Math.random()*3)]};
          pointer-events:none; z-index:9998;
          transition: transform 0.6s ease, opacity 0.6s ease;
          transform: translate(-50%,-50%);
        `;
        document.body.appendChild(b);
        const angle = (i/8)*360;
        const dist  = 50 + Math.random()*60;
        requestAnimationFrame(() => {
          b.style.transform = `translate(calc(-50% + ${Math.cos(angle*Math.PI/180)*dist}px), calc(-50% + ${Math.sin(angle*Math.PI/180)*dist}px))`;
          b.style.opacity   = '0';
        });
        setTimeout(() => b.remove(), 700);
      }
    });
  </script>
</body>
</html>
