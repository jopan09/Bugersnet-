<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>BUGERSNET — Community</title>
<link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Space+Grotesk:wght@500;700&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#050608;
    --neon:#00ffd5;
    --accent:#ff3b81;
    --muted:rgba(255,255,255,0.06);
  }
  *{box-sizing:border-box}
  html,body{height:100%;margin:0;font-family: 'Space Grotesk', system-ui, sans-serif;background:linear-gradient(180deg,#010205 0%, #061014 100%);color:#e6fff6;-webkit-font-smoothing:antialiased}
  .wrap{min-height:100vh;display:flex;flex-direction:column;align-items:center;justify-content:center;padding:28px;gap:22px;position:relative;overflow:hidden}

/* Intro overlay (5s) */
  .intro-overlay{
    position:fixed;inset:0;display:flex;align-items:center;justify-content:center;z-index:999;background:radial-gradient(circle at 30% 20%, rgba(0,255,160,.05), transparent 10%), rgba(0,0,0,.9);
    color:var(--neon);flex-direction:column;gap:18px;
  }
  .intro-title{font-family:'Share Tech Mono';font-size:84px;letter-spacing:6px;text-transform:uppercase;position:relative}
  .intro-title .g{display:inline-block;animation:glitchIntro 1.6s infinite linear}
  @keyframes glitchIntro{
    0%{transform:translate(0)}
    25%{transform:translate(-6px,-2px)}
    50%{transform:translate(6px,2px)}
    75%{transform:translate(-3px,1px)}
    100%{transform:translate(0)}
  }
  .intro-sub{font-size:14px;color:rgba(255,255,255,.6)}

/* Intro pulse progress bar */
  .progress{width:320px;height:8px;border-radius:6px;background:rgba(255,255,255,.04);overflow:hidden}
  .progress > i{display:block;height:100%;width:0;background:linear-gradient(90deg,var(--neon),var(--accent));animation:prog 5s linear forwards}
  @keyframes prog{to{width:100%}}

/* Main card */
  .card{width:min(1100px,96%);background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));border-radius:14px;padding:28px;display:flex;gap:22px;align-items:center;box-shadow:0 20px 60px rgba(0,0,0,.6);border:1px solid rgba(255,255,255,0.03);z-index:10;opacity:0;transform:translateY(8px);transition:opacity .6s ease, transform .6s ease}
  .card.visible{opacity:1;transform:translateY(0)}
  .left{flex:1}
  .right{width:320px;display:flex;flex-direction:column;gap:14px;align-items:center}

/* Big glitch title */
  .title{font-family:'Share Tech Mono', monospace;font-size:64px;letter-spacing:6px;text-transform:uppercase;color:var(--neon);position:relative;line-height:1}
  .glitch{position:relative;display:inline-block}
  .glitch::before,.glitch::after{content:attr(data-text);position:absolute;left:0;top:0;opacity:.9}
  .glitch::before{color:var(--accent);transform:translateX(-4px);mix-blend-mode:screen;animation:gtop 2.6s infinite linear}
  .glitch::after{color:#7affb2;transform:translateX(3px);mix-blend-mode:screen;animation:gbot 3.1s infinite linear}
  @keyframes gtop{0%{transform:translate(0,-1px)}20%{transform:translate(-8px,2px)}40%{transform:translate(6px,-1px)}60%{transform:translate(-2px,0)}100%{transform:translate(0,0)}}
  @keyframes gbot{0%{transform:translate(0,1px)}20%{transform:translate(8px,-2px)}40%{transform:translate(-6px,1px)}60%{transform:translate(2px,0)}100%{transform:translate(0,0)}}

/* description */
  .desc{color:rgba(230,255,246,.75);margin-top:8px;font-size:14px}

/* SOS button */
  .sos{
    background:linear-gradient(90deg,#ff2d6f,#ff8a5b);
    color:white;padding:14px 22px;border-radius:10px;border:0;font-weight:700;letter-spacing:1px;
    cursor:pointer;font-family:'Share Tech Mono';font-size:16px;box-shadow:0 10px 30px rgba(255,80,120,.12);transition:transform .12s ease;
  }
  .sos:active{transform:translateY(1px)}
  .hint{font-size:12px;color:rgba(255,255,255,.45);margin-top:8px;text-align:center}

/* modal */
  .modal-backdrop{position:fixed;inset:0;background:rgba(0,0,0,.6);display:flex;align-items:center;justify-content:center;z-index:60;backdrop-filter:blur(4px);display:none}
  .modal{width:min(420px,94%);background:#071015;padding:18px;border-radius:10px;border:1px solid rgba(255,255,255,.03)}
  .modal h3{margin:0 0 8px 0;font-family:'Share Tech Mono';color:var(--neon);font-size:18px}
  .modal p{margin:0 0 12px 0;color:rgba(255,255,255,.7);font-size:13px}
  .input-row{display:flex;gap:8px}
  input[type="password"]{flex:1;padding:10px 12px;border-radius:8px;border:1px solid rgba(255,255,255,.06);background:transparent;color:#dffef0;font-family:monospace}
  .btn{padding:10px 12px;border-radius:8px;border:0;background:var(--neon);color:#001;cursor:pointer;font-weight:700}
  .message{font-size:13px;margin-top:10px;color:#ff8b8b}
  .success{font-size:13px;margin-top:10px;color:#a7ffc9}

/* revealed link box */
  .reveal{width:100%;padding:14px;border-radius:8px;border:1px dashed rgba(255,255,255,.05);background:linear-gradient(180deg,rgba(255,255,255,.01),transparent);text-align:center}
  .linkbtn{display:inline-block;margin-top:10px;padding:10px 14px;border-radius:8px;background:#0b93ff;color:white;text-decoration:none;font-weight:700;font-family:'Share Tech Mono'}
  .small{font-size:12px;color:rgba(255,255,255,.45);margin-top:8px}

/* matrix canvas (subtle) */
  #matrix{position:absolute;inset:0;z-index:0;opacity:.12;mix-blend-mode:screen}

/* responsive */
  @media (max-width:880px){
    .card{flex-direction:column;align-items:stretch}
    .title{font-size:40px}
    .right{width:100%}
    .intro-title{font-size:44px}
  }
</style>
</head>
<body>
<div class="wrap" role="main">
  <canvas id="matrix"></canvas>

  <!-- Intro overlay (5 seconds) -->
  <div id="intro" class="intro-overlay" aria-hidden="false">
    <div class="intro-title" aria-hidden="true">
      <span class="g">BUGERS</span><span style="opacity:.5">NET</span>
    </div>
    <div class="intro-sub">official community — connecting curious minds</div>
    <div class="progress" aria-hidden="true"><i></i></div>
  </div>

  <div class="card" id="mainCard" role="region" aria-label="BUGERSNET intro" aria-hidden="true">
    <div class="left">
      <div class="title"><span class="glitch" data-text="BUGERSNET">BUGERSNET</span></div>
      <div class="desc">Welcome to BUGERSNET — a community for cyber aesthetics, tutorials and ethical projects. Use the SOS button below if you need quick official access.</div>
      <div style="height:14px"></div>
      <button id="sosBtn" class="sos" aria-haspopup="dialog" aria-controls="pwdModal">SOS</button>
      <div class="hint">Press SOS and enter the secret password to access the official IG link.</div>
    </div>

    <div class="right" aria-hidden="true">
      <div style="width:100%;padding:12px;border-radius:8px;background:rgba(255,255,255,0.02);text-align:center">
        <div style="font-family:'Share Tech Mono';font-size:13px;color:var(--neon)">OFFICIAL</div>
        <div style="font-size:18px;margin-top:8px">BUGERSNET</div>
        <div class="small">Community • Ethical • Creative</div>
        <div style="height:12px"></div>
        <div style="font-family:'Share Tech Mono';font-size:12px;color:rgba(255,255,255,.6)">Quick Access</div>
        <div style="height:8px"></div>
        <div class="reveal" id="revealBox">
          <div id="revealText">No special access active.</div>
          <a id="openLinkBtn" class="linkbtn" href="#" target="_blank" rel="noopener noreferrer" style="display:none">Open Instagram</a>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- modal template -->
<div id="pwdModal" class="modal-backdrop" role="dialog" aria-modal="true" aria-labelledby="modalTitle">
  <div class="modal">
    <h3 id="modalTitle">SOS — Enter Password</h3>
    <p>Enter the secret passcode to reveal the official BUGERSNET IG link.</p>
    <div class="input-row">
      <input id="pwdInput" type="password" placeholder="Password" aria-label="Password input" />
      <button id="pwdSubmit" class="btn">Enter</button>
    </div>
    <div id="msg" class="message" aria-live="polite" style="display:none"></div>
    <div id="success" class="success" style="display:none"></div>
    <div style="height:10px"></div>
    <div style="display:flex;gap:8px;justify-content:flex-end">
      <button id="cancelBtn" class="btn" style="background:var(--accent);color:#041">Cancel</button>
    </div>
  </div>
</div>

<script>
/* ---------- MATRIX (subtle) ---------- */
const canvas = document.getElementById('matrix');
const ctx = canvas.getContext('2d');
function resize(){ canvas.width = innerWidth; canvas.height = innerHeight; cols = Math.floor(canvas.width/14); drops = Array(cols).fill(1); }
let cols, drops;
resize();
addEventListener('resize', resize);
const letters = "01BUGERSNET@#$%&*()[]{}<>/\\|";
function draw(){
  ctx.fillStyle = "rgba(0,0,0,0.12)"; ctx.fillRect(0,0,canvas.width,canvas.height);
  ctx.fillStyle = "rgba(0,255,160,0.08)"; ctx.font = "14px 'Share Tech Mono', monospace";
  for(let i=0;i<drops.length;i++){
    const ch = letters[Math.floor(Math.random()*letters.length)];
    ctx.fillText(ch, i*14, drops[i]*14);
    if(drops[i]*14 > canvas.height && Math.random()>0.98) drops[i] = 0;
    drops[i]++;
  }
}
let matrixInterval = setInterval(draw, 55);

/* ---------- Intro control (5s) ---------- */
const intro = document.getElementById('intro');
const mainCard = document.getElementById('mainCard');

function endIntro(){
  intro.style.display = 'none';
  mainCard.classList.add('visible');
  mainCard.setAttribute('aria-hidden','false');
  // reduce matrix intensity on main UI
  canvas.style.opacity = '.12';
}
setTimeout(endIntro, 5000); // 5 seconds

/* ---------- SOS / PASSWORD logic (case-insensitive + auto-open) ---------- */
const SOS_BTN = document.getElementById('sosBtn');
const MODAL = document.getElementById('pwdModal');
const PWD = document.getElementById('pwdInput');
const SUBMIT = document.getElementById('pwdSubmit');
const CANCEL = document.getElementById('cancelBtn');
const MSG = document.getElementById('msg');
const SUCCESS = document.getElementById('success');
const REVEAL = document.getElementById('revealBox');
const REVEALTEXT = document.getElementById('revealText');
const OPENBTN = document.getElementById('openLinkBtn');

const CORRECT = "NYTHINGS"; // canonical password (we'll compare case-insensitively)
const IG_LINK = "https://www.instagram.com/bugersnetofficial?igsh=MzJ3YThpNW9tOXBr";

let attempts = 0;
let lockedUntil = 0;

function openModal(){
  MSG.style.display = 'none';
  SUCCESS.style.display = 'none';
  PWD.value = '';
  MODAL.style.display = 'flex';
  PWD.focus();
}
function closeModal(){ MODAL.style.display = 'none'; PWD.value=''; }

SOS_BTN.addEventListener('click', ()=>{
  const now = Date.now();
  if (lockedUntil && now < lockedUntil){
    const sec = Math.ceil((lockedUntil-now)/1000);
    REVEALTEXT.textContent = `Locked — try again in ${sec}s.`;
    return;
  }
  openModal();
});

CANCEL.addEventListener('click', closeModal);

SUBMIT.addEventListener('click', checkPwd);
PWD.addEventListener('keydown', (e)=>{ if(e.key === 'Enter') checkPwd(); });

function checkPwd(){
  const now = Date.now();
  if (lockedUntil && now < lockedUntil){
    MSG.style.display = 'block';
    MSG.textContent = 'Locked. Please wait.';
    return;
  }

  const value = (PWD.value || '').trim();
  if (value.toLowerCase() === CORRECT.toLowerCase()){
    // success
    MSG.style.display = 'none';
    SUCCESS.style.display = 'block';
    SUCCESS.textContent = 'Password accepted — opening official link...';
    revealLinkAndOpen();
    attempts = 0;
    setTimeout(closeModal, 600);
  } else {
    attempts++;
    MSG.style.display = 'block';
    MSG.textContent = `Wrong password (${attempts}).`;
    if (attempts >= 5){
      lockedUntil = Date.now() + 30000; // lock 30 seconds
      MSG.textContent = 'Too many wrong attempts. Locked for 30 seconds.';
      REVEALTEXT.textContent = 'Locked — wait 30s.';
      closeModal();
    }
  }
}

/* reveal the instagram link in the right panel and auto-open */
function revealLink(){
  REVEALTEXT.innerHTML = `Access granted — official IG unlocked.`;
  OPENBTN.style.display = 'inline-block';
  OPENBTN.href = IG_LINK;
}

/* Open link in new tab — called after successful enter (user gesture) */
function revealLinkAndOpen(){
  revealLink();
  // open in a new tab (should be permitted since triggered by button click)
  try {
    window.open(IG_LINK, '_blank', 'noopener');
  } catch (e) {
    // fallback: show the visible link so the user can click
    console.warn('Auto-open blocked:', e);
  }
}

/* optional: keyboard shortcut (Shift+S) */
document.addEventListener('keydown', (e)=>{
  if (e.shiftKey && e.key.toLowerCase() === 's'){
    SOS_BTN.click();
  }
});

/* Accessibility: close modal on backdrop click or Escape */
MODAL.addEventListener('click', (e)=>{
  if (e.target === MODAL) closeModal();
});
document.addEventListener('keydown', (e)=>{ if(e.key === 'Escape') closeModal(); });

/* Initialize reveal text */
REVEALTEXT.textContent = 'No special access active.';

/* Clean up matrix interval on page unload */
window.addEventListener('beforeunload', ()=>{ clearInterval(matrixInterval); });

</script>
</body>
</html>
