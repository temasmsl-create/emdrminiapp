<script src="https://telegram.org/js/telegram-web-app.js"></script>

<style>
  :root{
    --bg:#fbfcfe; --card:#ffffff; --accent-1:#173e9b; --accent-2:#6fa8ff; --text:#0f1724;
    --safe-top: env(safe-area-inset-top); --safe-bottom: env(safe-area-inset-bottom);
  }
  html,body{height:100%;margin:0;font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial;}
  #appOverlay{position:fixed; inset:0; z-index:99999; background: linear-gradient(180deg,#f8fbff 0%, #eef6ff 100%); display:flex; flex-direction:column; -webkit-tap-highlight-color: transparent;}
  .app-header{height:62px; padding-top:calc(var(--safe-top) + 6px); padding-left:12px; padding-right:12px; display:flex; align-items:center; gap:12px; background:linear-gradient(90deg,var(--accent-1), #2f6bff); color:#fff; box-shadow:0 2px 10px rgba(15,30,80,0.12);}
  .app-title{font-weight:800;font-size:16px;flex:1; text-transform:uppercase; letter-spacing:0.6px}
  .app-logo{height:36px; width:36px; border-radius:8px; background:#fff; padding:4px; box-sizing:border-box; display:flex; align-items:center; justify-content:center;}
  .app-close{background: rgba(255,255,255,0.12); padding:6px 10px; border-radius:10px; color:#fff; font-weight:700; cursor:pointer; border:0}
  .app-body{flex:1; overflow:auto; padding:18px; padding-bottom: calc(22px + var(--safe-bottom)); color:var(--text)}
  .card{background:var(--card); border-radius:14px; padding:14px; box-shadow:0 8px 24px rgba(27,46,89,0.06); margin-bottom:14px}
  h2{margin:0 0 6px; font-size:18px}
  p.small{margin:0 0 12px; color:#475569; font-size:14px}
  .row{display:flex;align-items:center;gap:12px;margin-bottom:12px}
  label{min-width:96px;font-weight:700;color:var(--text); font-size:14px}
  .range-wrap{flex:1; display:flex; align-items:center; gap:10px}
  input[type=range]{-webkit-appearance:none; width:100%; height:28px; background:transparent}
  input[type=range]::-webkit-slider-runnable-track{height:8px; border-radius:999px; background:linear-gradient(90deg, #e6f0ff, #eaf4ff)}
  input[type=range]::-webkit-slider-thumb{-webkit-appearance:none; margin-top:-8px; width:22px; height:22px; border-radius:999px; background:var(--accent-2); box-shadow:0 8px 20px rgba(47,107,255,0.18); border:2px solid var(--accent-1)}
  .value{width:46px;text-align:center;font-weight:800}
  .controls{display:flex;gap:10px;margin-top:6px}
  .btn{flex:1;padding:12px;border-radius:12px;border:0;font-weight:800;background:var(--accent-1);color:#fff;font-size:16px;cursor:pointer;box-shadow:0 8px 20px rgba(47,107,255,0.18)}
  .btn.secondary{background:#f6a54a; box-shadow:0 8px 20px rgba(246,165,74,0.14)}
  .btn.danger{background:#ff5a5a; box-shadow:0 8px 20px rgba(255,90,90,0.12)}
  #play-area{position:relative;height:270px;margin-top:12px;background:linear-gradient(180deg,#ffffff,#f6fbff);border-radius:12px;overflow:hidden;border:1px solid rgba(14,40,85,0.04)}
  #emdr-ball{position:absolute; top:50%; transform:translateY(-50%); display:none; border-radius:50%; background: radial-gradient(circle at 30% 30%, #9bbaff 0%, var(--accent-2) 45%, var(--accent-1) 100%); box-shadow:0 12px 28px rgba(31,76,189,0.16), inset 0 -8px 18px rgba(0,0,0,0.06)}
  .counter{margin-top:10px;font-weight:800;color:var(--text)}
  /* dialog modal */
  .dialog-overlay{position:absolute; inset:0; background:rgba(8,12,24,0.28); display:flex; align-items:center; justify-content:center; z-index:5}
  .dialog{width:90%; max-width:720px; background:#fff; border-radius:12px; padding:16px; box-shadow:0 12px 36px rgba(8,18,48,0.3); text-align:center}
  .dialog p{margin:10px 0; font-weight:700}
  .dialog textarea{width:100%; min-height:92px; padding:10px; border-radius:8px; border:1px solid #e6eefb; resize:vertical}
  .dialog .send-btn{margin-top:8px; padding:10px 14px; border-radius:10px; background:var(--accent-1); color:#fff; border:0; font-weight:800; cursor:pointer}
  @media (min-width:900px){ #appOverlay{align-items:center} .app-body{max-width:780px} }
</style>

<div id="appOverlay" role="application" aria-label="EMDR mini app">
  <div class="app-header">
    <div class="app-logo"><img id="logoImg" src="" alt="logo" style="height:100%; width:100%; object-fit:contain"></div>
    <div class="app-title">EMDR QAZAQSTAN</div>
    <button class="app-close" id="closeApp">Закрыть</button>
  </div>

  <div class="app-body">
    <div class="card">
      <h2>EMDR — мини-сессия</h2>
      <p class="small">Выставь параметры слайдерами и нажми <strong>Старт</strong>. Следи взглядом за мячиком.</p>

      <div class="row">
        <label>Скорость</label>
        <div class="range-wrap">
          <input id="speedRange" type="range" min="1" max="20" value="10">
          <div class="value" id="speedVal">10</div>
        </div>
      </div>

      <div class="row">
        <label>Размер</label>
        <div class="range-wrap">
          <input id="sizeRange" type="range" min="1" max="5" value="3">
          <div class="value" id="sizeVal">3</div>
        </div>
      </div>

      <div class="row">
        <label>Сеты</label>
        <div class="range-wrap">
          <input id="setsRange" type="range" min="1" max="30" value="10">
          <div class="value" id="setsVal">10</div>
        </div>
      </div>

      <div class="controls">
        <button id="startBtn" class="btn">Старт</button>
        <button id="pauseBtn" class="btn secondary" style="display:none">Пауза</button>
        <button id="stopBtn" class="btn danger" style="display:none">Стоп</button>
      </div>

      <div class="counter" id="set-counter"></div>
    </div>

    <div id="play-area" class="card" aria-hidden="false">
      <div id="emdr-ball" aria-hidden="true"></div>
      <!-- dialog overlay will be inserted here dynamically -->
    </div>
  </div>
</div>

<script>
(function(){
  // ====== PUT YOUR image URL here (https) ======
  const LOGO_URL = "https://static.tildacdn.pro/tild6161-3336-4364-b861-663461373464/EMDR____.png";
  const logoImg = document.getElementById('logoImg');
  if(LOGO_URL && LOGO_URL.startsWith('http')){ logoImg.src = LOGO_URL; logoImg.onerror = ()=> { logoImg.style.display='none'; console.warn('Logo failed to load'); } } else { logoImg.style.display='none'; console.warn('Logo URL not set — hide logo'); }

  // Telegram
  let tg=false;
  try{ tg = window.Telegram.WebApp; tg.expand(); }catch(e){}

  // DOM refs
  const speedRange = document.getElementById('speedRange');
  const sizeRange  = document.getElementById('sizeRange');
  const setsRange  = document.getElementById('setsRange');
  const speedVal   = document.getElementById('speedVal');
  const sizeVal    = document.getElementById('sizeVal');
  const setsVal    = document.getElementById('setsVal');
  const startBtn = document.getElementById('startBtn');
  const pauseBtn = document.getElementById('pauseBtn');
  const stopBtn  = document.getElementById('stopBtn');
  const ball = document.getElementById('emdr-ball');
  const playArea = document.getElementById('play-area');
  const counterEl = document.getElementById('set-counter');
  const closeBtn = document.getElementById('closeApp');

  closeBtn.onclick = ()=> { try{ tg && tg.close(); }catch(e){ window.history.back(); } };

  // labels
  speedRange.oninput = ()=> speedVal.innerText = speedRange.value;
  sizeRange.oninput  = ()=> sizeVal.innerText  = sizeRange.value;
  setsRange.oninput  = ()=> setsVal.innerText  = setsRange.value;

  // state
  let animationId=null, vx=0, direction=1, setsLeft=0, paused=false, posX=0, fullWidth=0, ballPx=48, audioCtx=null;
  let dialogShown = false; // ONLY ONCE per app launch
  let dialogActive = false;
  let resumeAfterDialog = false; // if stop triggered dialog, resume after user sends

  // sound: rubbery bounce
  function playRubberBounce(){
    try{
      if(!audioCtx) audioCtx = new (window.AudioContext||window.webkitAudioContext)();
      const now = audioCtx.currentTime;
      const o1 = audioCtx.createOscillator(); o1.type='triangle'; o1.frequency.value = 260 + Math.random()*120;
      const o2 = audioCtx.createOscillator(); o2.type='sine'; o2.frequency.value = o1.frequency.value * 1.6;
      const filter = audioCtx.createBiquadFilter(); filter.type = 'lowpass'; filter.frequency.value = 1200;
      const g = audioCtx.createGain(); g.gain.value = 0.0001;
      o1.connect(filter); o2.connect(filter); filter.connect(g); g.connect(audioCtx.destination);
      g.gain.linearRampToValueAtTime(0.18, now + 0.003);
      g.gain.exponentialRampToValueAtTime(0.0001, now + 0.22);
      o1.frequency.setValueAtTime(o1.frequency.value * 1.15, now);
      o1.frequency.exponentialRampToValueAtTime(o1.frequency.value * 0.9, now + 0.12);
      o1.start(now); o2.start(now); o1.stop(now + 0.26); o2.stop(now + 0.26);
    }catch(e){ console.warn(e) }
  }

  // layout recalc
  function recalc(){ ballPx = Number(sizeRange.value) * 18 + 12; ball.style.width = ball.style.height = ballPx + 'px'; fullWidth = Math.max(0, playArea.clientWidth - ballPx); posX = Math.min(Math.max(0, posX), fullWidth); ball.style.left = posX + 'px'; }
  window.addEventListener('resize', ()=> recalc());
  window.addEventListener('orientationchange', ()=> setTimeout(recalc,120));

  // core control
  function startSession(){
    const speed = Number(speedRange.value); // 1..20
    vx = 0.8 * speed; // slider max maps to reasonable vx
    setsLeft = Number(setsRange.value);
    posX = 0; direction = 1; paused = false;
    startBtn.style.display='none'; pauseBtn.style.display='inline-block'; stopBtn.style.display='inline-block';
    pauseBtn.innerText='Пауза';
    ball.style.display='block'; recalc(); updateCounter();
    if(animationId) cancelAnimationFrame(animationId);
    animationLoop();
  }
  function pauseToggle(){ // pause button: second press should NOT trigger dialog again
    paused = !paused;
    pauseBtn.innerText = paused ? 'Продолжить' : 'Пауза';
    // if paused by user and dialog not shown yet and dialog not active, show dialog now (only once)
    if(paused && !dialogShown && !dialogActive){
      // but user asked earlier: second time pressing pause/stop — no dialog. Here show on first pause only.
      showDialogFlow({trigger:'pause'});
    }
  }

  function stopSession(){ // stop pressed by user
    // stop animation, keep state so we can resume after dialog if needed
    if(animationId) cancelAnimationFrame(animationId);
    animationId = null;
    ball.style.display='none';
    startBtn.style.display='inline-block';
    pauseBtn.style.display='none';
    stopBtn.style.display='none';
    // show dialog if not yet shown
    if(!dialogShown && !dialogActive){
      resumeAfterDialog = true; // resume after user reply
      showDialogFlow({trigger:'stop'});
    }
  }

  function endSessionNatural(){ // used when setsLeft reaches 0
    // session finished naturally
    if(animationId) cancelAnimationFrame(animationId);
    animationId = null;
    ball.style.display='none';
    startBtn.style.display='inline-block';
    pauseBtn.style.display='none';
    stopBtn.style.display='none';
    // show dialog if not yet shown
    if(!dialogShown && !dialogActive){
      resumeAfterDialog = false;
      showDialogFlow({trigger:'natural'});
    }
  }

  function animationLoop(){
    if(!paused){
      posX += vx * direction;
      if(posX >= fullWidth){
        posX = fullWidth; direction = -1; onBounce();
      }
      if(posX <= 0){
        posX = 0; direction = 1; setsLeft = Math.max(0, setsLeft - 1); updateCounter(); onBounce();
        if(setsLeft <= 0){ endSessionNatural(); return; }
      }
      ball.style.left = posX + 'px';
    }
    animationId = requestAnimationFrame(animationLoop);
  }

  function onBounce(){ playRubberBounce(); try{ if(navigator.vibrate) navigator.vibrate(35);}catch(e){} }

  function updateCounter(){ counterEl.innerHTML = 'Осталось сетов: <b>'+setsLeft+'</b>'; }

  // init bindings
  startBtn.addEventListener('click', startSession);
  pauseBtn.addEventListener('click', pauseToggle);
  stopBtn.addEventListener('click', stopSession);

  // DIALOG FLOW (single-run)
  function showDialogFlow(meta={trigger:'manual'}) {
    dialogActive = true;
    dialogShown = true; // ensure only once
    // overlay element
    const overlay = document.createElement('div');
    overlay.className = 'dialog-overlay';
    overlay.innerHTML = `
      <div class="dialog">
        <p id="dlg-text">Что удалось заметить?</p>
        <div id="dlg-input-wrap" style="display:none;">
          <textarea id="dlg-textarea" placeholder="Опишите ваши ощущения, заметки..." ></textarea>
          <button id="dlg-send" class="send-btn">Отправить</button>
        </div>
      </div>
    `;
    playArea.appendChild(overlay);

    // step 1: show prompt 2s
    const textEl = overlay.querySelector('#dlg-text');
    const inputWrap = overlay.querySelector('#dlg-input-wrap');
    const textarea = overlay.querySelector('#dlg-textarea');
    const sendBtn = overlay.querySelector('#dlg-send');

    setTimeout(()=>{ // show textarea
      textEl.style.display = 'none';
      inputWrap.style.display = 'block';
      textarea.focus();
    }, 2000);

    // send handler
    sendBtn.onclick = ()=> {
      const val = textarea.value.trim();
      // you can send val to your bot here via tg API or fetch webhook
      // example: tg.sendData(JSON.stringify({note: val})); // optional
      // show confirmation "Хорошо. Заметь это" for 2s then remove dialog
      inputWrap.style.display = 'none';
      textEl.innerText = 'Хорошо. Заметь это';
      textEl.style.display = 'block';
      setTimeout(()=> {
        overlay.remove();
        dialogActive = false;
        // if resumeAfterDialog true -> resume animation from current pos
        if(resumeAfterDialog){
          resumeAfterDialog = false;
          // resume UI and animation
          startBtn.style.display='none'; pauseBtn.style.display='inline-block'; stopBtn.style.display='inline-block';
          paused = false;
          ball.style.display='block';
          if(animationId) cancelAnimationFrame(animationId);
          animationLoop();
        } else {
          // do nothing — session remains ended
        }
      }, 2000);
    };
  }

  // final init
  recalc();

  // Expose function to programmatically trigger dialog (if needed)
  window.__emdr_show_dialog = ()=> { if(!dialogShown && !dialogActive) showDialogFlow(); };

})();
</script>
