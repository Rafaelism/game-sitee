[Pocket Arcade.html](https://github.com/user-attachments/files/35403738/Pocket.Arcade.html)
<!doctype html><html><head><meta charset=utf8><meta name=viewport content="width=device-width,initial-scale=1,viewport-fit=cover"><style>:root{color-scheme:light;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}html{scroll-padding-top:env(safe-area-inset-top,0px)}body{margin:0;padding:0;font:14px -apple-system,BlinkMacSystemFont,sans-serif;background:#fff;color:#000}img{max-width:100%}[hidden]:not([hidden=until-found i]){display:none!important}</style></head><body>
<title>Pocket Arcade</title>
<meta name="claude:viewport-fit-sides" content="cover">
<style>
/* Layout: one centered console. Header, tab strip, one game panel at a time, scoreboard beneath. */
:root{
  color-scheme:dark;
  --bg:#15122b;
  --panel:#201c42;
  --panel-2:#2a2552;
  --line:#3a3468;
  --fg:#f1eefc;
  --muted:#a7a0cf;
  --amber:#ffb347;
  --teal:#3ddbc4;
  --pink:#ff6b9a;
  --font-display:"Space Grotesk",system-ui,sans-serif;
  --font-body:"Inter",system-ui,sans-serif;
  --font-mono:"Space Mono",ui-monospace,monospace;
}
*{box-sizing:border-box}
html,body{min-height:100%}
body{
  background:var(--bg);color:var(--fg);font-family:var(--font-body);font-size:16px;line-height:1.5;
  padding-block:24px 48px;
  padding-left:max(16px,env(safe-area-inset-left,0px));
  padding-right:max(16px,env(safe-area-inset-right,0px));
}
.wrap{max-width:560px;margin:0 auto;display:flex;flex-direction:column;gap:20px;min-width:0}
header{display:flex;justify-content:space-between;align-items:flex-end;gap:12px;flex-wrap:wrap}
h1{font-family:var(--font-display);font-size:2rem;line-height:1.05;margin:0;letter-spacing:-.02em;text-wrap:balance}
h1 span{color:var(--amber)}
.tag{margin:0;color:var(--muted);font-size:.9rem}
nav{display:grid;grid-template-columns:repeat(3,1fr);gap:8px}
nav a{
  font-family:var(--font-display);font-weight:600;text-align:center;text-decoration:none;color:var(--muted);
  background:var(--panel);border:2px solid var(--line);border-radius:10px;padding:10px 6px;
}
nav a:hover{color:var(--fg);border-color:var(--muted)}
nav a[aria-current="page"]{color:#1b1500;background:var(--amber);border-color:var(--amber)}
a:focus-visible,button:focus-visible{outline:3px solid var(--teal);outline-offset:2px}
section.game{display:none;flex-direction:column;gap:14px}
section.game.on{display:flex}
.stats{display:flex;gap:10px}
.stat{flex:1;background:var(--panel);border:2px solid var(--line);border-radius:10px;padding:8px 12px;min-width:0}
.stat small{display:block;color:var(--muted);font-size:.7rem;letter-spacing:.12em;text-transform:uppercase}
.stat b{font-family:var(--font-mono);font-size:1.5rem;font-variant-numeric:tabular-nums}
.how{margin:0;color:var(--muted);font-size:.9rem}
.btn{
  font-family:var(--font-display);font-weight:700;font-size:1rem;color:#06231e;background:var(--teal);
  border:0;border-radius:10px;padding:12px 18px;cursor:pointer;
}
.btn:active{transform:translateY(1px)}
.row{display:flex;gap:10px;align-items:center;flex-wrap:wrap}
/* snake */
.board{position:relative;width:100%;aspect-ratio:1;max-width:100%;border:2px solid var(--line);border-radius:12px;overflow:hidden;background:var(--panel);touch-action:none}
.board canvas{display:block;width:100%;height:100%}
.veil{position:absolute;inset:0;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:10px;background:rgba(21,18,43,.78);text-align:center;padding:16px}
.veil[hidden]{display:none!important}
.veil h2{font-family:var(--font-display);margin:0;font-size:1.6rem}
.pad{display:grid;grid-template-columns:repeat(3,64px);grid-template-rows:repeat(2,52px);gap:6px;justify-content:center}
.pad button{background:var(--panel-2);color:var(--fg);border:2px solid var(--line);border-radius:10px;font-size:1.3rem;cursor:pointer;touch-action:manipulation}
.pad button:active{background:var(--line)}
.pad .u{grid-column:2;grid-row:1}.pad .l{grid-column:1;grid-row:2}.pad .d{grid-column:2;grid-row:2}.pad .r{grid-column:3;grid-row:2}
@media (pointer:fine){.pad{display:none}}
/* memory */
.cards{display:grid;grid-template-columns:repeat(4,1fr);gap:8px}
.card{
  aspect-ratio:1;border-radius:12px;border:2px solid var(--line);background:var(--panel-2);cursor:pointer;
  font-size:clamp(1.6rem,8vw,2.6rem);font-family:var(--font-display);color:transparent;display:flex;align-items:center;justify-content:center;padding:0;
}
.card:hover{border-color:var(--muted)}
.card.up{background:var(--panel);color:var(--c)}
.card.done{background:var(--panel);color:var(--c);border-color:var(--c);opacity:.65;cursor:default}
/* reflex */
.pane{
  width:100%;min-height:220px;border-radius:14px;border:2px solid var(--line);cursor:pointer;
  font-family:var(--font-display);font-size:1.5rem;font-weight:700;color:var(--fg);background:var(--panel-2);padding:20px;text-wrap:balance;
}
.pane.wait{background:#5a1f3c;border-color:var(--pink)}
.pane.go{background:var(--teal);border-color:var(--teal);color:#06231e}
.pane small{display:block;font-family:var(--font-body);font-weight:400;font-size:.9rem;margin-top:6px;opacity:.85}
.hist{display:flex;gap:6px;flex-wrap:wrap;font-family:var(--font-mono);font-size:.85rem;color:var(--muted);min-height:1.5em}
.hist span{background:var(--panel);border:1px solid var(--line);border-radius:6px;padding:2px 8px}
@media (prefers-reduced-motion:no-preference){.card,.pane,nav a{transition:background .15s,border-color .15s,color .15s}}
</style>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600&family=Space+Grotesk:wght@500;700&family=Space+Mono:wght@700&display=swap">

<div class="wrap">
  <header>
    <h1>Pocket <span>Arcade</span></h1>
    <p class="tag">Three quick games. Beat your best.</p>
  </header>
  <nav aria-label="Games">
    <a href="#snake" data-g="snake">Snake</a>
    <a href="#memory" data-g="memory">Memory</a>
    <a href="#reflex" data-g="reflex">Reflex</a>
  </nav>

  <section class="game" id="g-snake">
    <div class="stats">
      <div class="stat"><small>Score</small><b id="sn-score">0</b></div>
      <div class="stat"><small>Best</small><b id="sn-best">0</b></div>
    </div>
    <div class="board" id="sn-board">
      <canvas id="sn-cv" width="400" height="400"></canvas>
      <div class="veil" id="sn-veil">
        <h2 id="sn-title">Snake</h2>
        <p class="how" id="sn-msg">Arrow keys or WASD. On a phone, swipe or use the pad.</p>
        <button class="btn" id="sn-start" type="button">Start</button>
      </div>
    </div>
    <div class="pad" aria-label="Direction pad">
      <button class="u" type="button" data-d="u" aria-label="Up">▲</button>
      <button class="l" type="button" data-d="l" aria-label="Left">◀</button>
      <button class="d" type="button" data-d="d" aria-label="Down">▼</button>
      <button class="r" type="button" data-d="r" aria-label="Right">▶</button>
    </div>
  </section>

  <section class="game" id="g-memory">
    <div class="stats">
      <div class="stat"><small>Moves</small><b id="me-moves">0</b></div>
      <div class="stat"><small>Time</small><b id="me-time">0s</b></div>
      <div class="stat"><small>Best</small><b id="me-best">–</b></div>
    </div>
    <div class="cards" id="me-cards"></div>
    <div class="row">
      <button class="btn" id="me-new" type="button">New game</button>
      <p class="how" id="me-msg">Flip two cards. Match all eight pairs in the fewest moves.</p>
    </div>
  </section>

  <section class="game" id="g-reflex">
    <div class="stats">
      <div class="stat"><small>Last</small><b id="rf-last">–</b></div>
      <div class="stat"><small>Average</small><b id="rf-avg">–</b></div>
      <div class="stat"><small>Best</small><b id="rf-best">–</b></div>
    </div>
    <button class="pane" id="rf-pane" type="button">Click to start<small>Wait for green, then click as fast as you can.</small></button>
    <div class="hist" id="rf-hist" aria-label="Recent times"></div>
  </section>
</div>

<script>
(function(){
  var $=function(id){return document.getElementById(id)};
  function load(k){try{return localStorage.getItem('pa-'+k)}catch(e){return null}}
  function save(k,v){try{localStorage.setItem('pa-'+k,String(v))}catch(e){}}
  var current='snake', GAMES=['snake','memory','reflex'];

  /* ---------- routing ---------- */
  function show(){
    var h=(location.hash||'').slice(1);
    current=GAMES.indexOf(h)>-1?h:'snake';
    GAMES.forEach(function(g){
      $('g-'+g).classList.toggle('on',g===current);
      var a=document.querySelector('nav a[data-g="'+g+'"]');
      if(g===current)a.setAttribute('aria-current','page');else a.removeAttribute('aria-current');
    });
    if(current!=='snake')snake.stop();
    if(current!=='reflex')reflex.cancel();
  }

  /* ---------- snake ---------- */
  var snake=(function(){
    var cv=$('sn-cv'),cx=cv.getContext('2d'),N=20,S=cv.width/N;
    var body,dir,next,food,score,tm,running=false,best=+load('snake')||0;
    $('sn-best').textContent=best;
    function rnd(n){return Math.floor(Math.random()*n)}
    function place(){do{food={x:rnd(N),y:rnd(N)}}while(body.some(function(s){return s.x===food.x&&s.y===food.y}))}
    function reset(){
      body=[{x:10,y:10},{x:9,y:10},{x:8,y:10}];dir={x:1,y:0};next=dir;score=0;
      $('sn-score').textContent=0;place();draw();
    }
    function col(n){return getComputedStyle(document.documentElement).getPropertyValue(n).trim()}
    function draw(){
      cx.fillStyle=col('--panel');cx.fillRect(0,0,400,400);
      cx.fillStyle=col('--panel-2');
      for(var i=0;i<N;i++)for(var j=0;j<N;j++)if((i+j)%2===0)cx.fillRect(i*S,j*S,S,S);
      cx.fillStyle=col('--pink');
      cx.beginPath();cx.arc(food.x*S+S/2,food.y*S+S/2,S*.36,0,7);cx.fill();
      body.forEach(function(s,i){
        cx.fillStyle=i===0?col('--amber'):col('--teal');
        cx.fillRect(s.x*S+1,s.y*S+1,S-2,S-2);
      });
    }
    function loop(){
      dir=next;
      var h={x:body[0].x+dir.x,y:body[0].y+dir.y};
      if(h.x<0||h.y<0||h.x>=N||h.y>=N||body.some(function(s){return s.x===h.x&&s.y===h.y})){return end()}
      body.unshift(h);
      if(h.x===food.x&&h.y===food.y){score++;$('sn-score').textContent=score;place()}else body.pop();
      draw();
      tm=setTimeout(loop,Math.max(60,125-score*3));
    }
    function start(){
      reset();running=true;$('sn-veil').hidden=true;clearTimeout(tm);tm=setTimeout(loop,200);
    }
    function stop(){running=false;clearTimeout(tm);}
    function end(){
      stop();
      var nb=score>best;if(nb){best=score;save('snake',best);$('sn-best').textContent=best}
      $('sn-title').textContent='Game over';
      $('sn-msg').textContent='Score '+score+(nb&&score>0?'. New best!':'.');
      $('sn-start').textContent='Play again';
      $('sn-veil').hidden=false;
    }
    function turn(x,y){
      if(!running)return;
      if(x===-dir.x&&y===-dir.y)return;
      next={x:x,y:y};
    }
    var MAP={ArrowUp:[0,-1],w:[0,-1],ArrowDown:[0,1],s:[0,1],ArrowLeft:[-1,0],a:[-1,0],ArrowRight:[1,0],d:[1,0]};
    document.addEventListener('keydown',function(e){
      if(current!=='snake')return;
      var m=MAP[e.key];
      if(m){e.preventDefault();turn(m[0],m[1])}
    });
    var D={u:[0,-1],d:[0,1],l:[-1,0],r:[1,0]};
    document.querySelectorAll('.pad button').forEach(function(b){
      b.addEventListener('click',function(){var m=D[b.dataset.d];turn(m[0],m[1])});
    });
    var tx=0,ty=0;
    var bd=$('sn-board');
    bd.addEventListener('touchstart',function(e){tx=e.touches[0].clientX;ty=e.touches[0].clientY},{passive:true});
    bd.addEventListener('touchend',function(e){
      var t=e.changedTouches[0],dx=t.clientX-tx,dy=t.clientY-ty;
      if(Math.max(Math.abs(dx),Math.abs(dy))<24)return;
      if(Math.abs(dx)>Math.abs(dy))turn(dx>0?1:-1,0);else turn(0,dy>0?1:-1);
    });
    $('sn-start').addEventListener('click',start);
    reset();
    return {stop:function(){if(running){stop();$('sn-title').textContent='Paused';$('sn-msg').textContent='Press Start for a new run.';$('sn-start').textContent='Start';$('sn-veil').hidden=false}}};
  })();

  /* ---------- memory ---------- */
  (function(){
    var SYM=[['●','#ffb347'],['▲','#3ddbc4'],['■','#ff6b9a'],['◆','#8fb7ff'],['★','#ffd966'],['✚','#7be07b'],['⬢','#c79bff'],['✸','#ff9d5c']];
    var cards=$('me-cards'),first=null,lock=false,moves=0,matched=0,t0=0,tick=null,best=+load('memory')||0;
    if(best)$('me-best').textContent=best;
    function shuffle(a){for(var i=a.length-1;i>0;i--){var j=Math.floor(Math.random()*(i+1));var t=a[i];a[i]=a[j];a[j]=t}return a}
    function setMoves(n){moves=n;$('me-moves').textContent=n}
    function newGame(){
      clearInterval(tick);t0=0;first=null;lock=false;matched=0;setMoves(0);$('me-time').textContent='0s';
      $('me-msg').textContent='Flip two cards. Match all eight pairs in the fewest moves.';
      cards.innerHTML='';
      shuffle(SYM.concat(SYM)).forEach(function(s,i){
        var b=document.createElement('button');
        b.type='button';b.className='card';b.textContent=s[0];b.style.setProperty('--c',s[1]);b.dataset.s=s[0];
        b.setAttribute('aria-label','Card '+(i+1)+', face down');
        b.addEventListener('click',function(){flip(b)});
        cards.appendChild(b);
      });
    }
    function flip(b){
      if(lock||b.classList.contains('up')||b.classList.contains('done'))return;
      if(!t0){t0=Date.now();tick=setInterval(function(){$('me-time').textContent=Math.floor((Date.now()-t0)/1000)+'s'},500)}
      b.classList.add('up');b.setAttribute('aria-label','Card '+b.dataset.s);
      if(!first){first=b;return}
      setMoves(moves+1);
      if(first.dataset.s===b.dataset.s){
        first.className=b.className='card done';first=null;matched++;
        if(matched===8)win();
      }else{
        lock=true;var a=first;first=null;
        setTimeout(function(){a.classList.remove('up');b.classList.remove('up');lock=false},700);
      }
    }
    function win(){
      clearInterval(tick);
      var secs=Math.floor((Date.now()-t0)/1000),nb=!best||moves<best;
      if(nb){best=moves;save('memory',best);$('me-best').textContent=best}
      $('me-msg').textContent='Cleared in '+moves+' moves and '+secs+'s'+(nb?'. New best!':'.');
    }
    $('me-new').addEventListener('click',newGame);
    newGame();
  })();

  /* ---------- reflex ---------- */
  var reflex=(function(){
    var pane=$('rf-pane'),state='idle',tm=null,t0=0,times=[],best=+load('reflex')||0;
    if(best)$('rf-best').textContent=best+' ms';
    function set(cls,html){pane.className='pane '+cls;pane.innerHTML=html}
    function render(){
      $('rf-hist').innerHTML=times.slice(-5).map(function(t){return '<span>'+t+' ms</span>'}).join('');
      if(times.length){
        var last=times.slice(-5),avg=Math.round(last.reduce(function(a,b){return a+b},0)/last.length);
        $('rf-last').textContent=times[times.length-1]+' ms';$('rf-avg').textContent=avg+' ms';
      }
    }
    function arm(){
      state='wait';set('wait','Wait for green…');
      tm=setTimeout(function(){state='go';t0=performance.now();set('go','Click!')},1200+Math.random()*2800);
    }
    pane.addEventListener('click',function(){
      if(state==='idle'||state==='result'||state==='early'){arm()}
      else if(state==='wait'){clearTimeout(tm);state='early';set('','Too soon<small>Click to try again.</small>')}
      else if(state==='go'){
        var ms=Math.round(performance.now()-t0);times.push(ms);state='result';
        if(!best||ms<best){best=ms;save('reflex',best);$('rf-best').textContent=best+' ms'}
        set('',ms+' ms<small>Click to go again.</small>');render();
      }
    });
    return {cancel:function(){clearTimeout(tm);if(state==='wait'||state==='go'){state='idle';set('','Click to start<small>Wait for green, then click as fast as you can.</small>')}}};
  })();

  window.addEventListener('hashchange',show);
  show();
})();
</script>

</body></html>
