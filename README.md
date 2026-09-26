```html
<!DOCTYPE html>
<html lang="pl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
<title>Neon Dash</title>

<style>
*{
  box-sizing:border-box;
  -webkit-tap-highlight-color:transparent;
}

html,body{
  margin:0;
  width:100%;
  height:100%;
  overflow:hidden;
  background:#050510;
  color:white;
  font-family:Arial,sans-serif;
}

button{
  font-family:Arial,sans-serif;
  border:0;
  cursor:pointer;
}

#home{
  position:fixed;
  inset:0;
  overflow:auto;
  background:
    radial-gradient(circle at 50% 20%,#202050,#080817 55%,#020207);
  text-align:center;
}

.logo{
  margin:0;
  padding-top:70px;
  font-size:clamp(45px,10vw,90px);
  font-weight:900;
  letter-spacing:5px;
  color:white;
  text-shadow:
    0 0 10px #00ffff,
    0 0 30px #00ffff,
    0 0 60px #8a2cff;
}

.subtitle{
  color:#aaa;
  margin:10px 15px 30px;
}

.mainButton{
  padding:16px 45px;
  border-radius:14px;
  background:#00ffff;
  color:#020207;
  font-size:20px;
  font-weight:900;
  box-shadow:0 0 25px #00ffff;
}

.homeStats{
  margin:40px auto;
  max-width:700px;
  display:flex;
  justify-content:center;
  gap:12px;
  flex-wrap:wrap;
}

.stat{
  min-width:140px;
  padding:18px;
  border-radius:15px;
  background:#111126;
  border:1px solid #29294c;
}

.stat b{
  display:block;
  font-size:28px;
  color:#00ffff;
}

.levelList{
  width:min(1000px,94%);
  margin:20px auto 60px;
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(200px,1fr));
  gap:12px;
}

.levelCard{
  padding:18px;
  border-radius:15px;
  background:#101021;
  border:1px solid #303050;
  text-align:left;
}

.levelCard.locked{
  opacity:.4;
}

.levelName{
  font-size:18px;
  font-weight:bold;
  margin:6px 0;
}

.levelInfo{
  color:#999;
  font-size:13px;
}

#game{
  position:fixed;
  inset:0;
  display:none;
  background:#020207;
}

canvas{
  position:absolute;
  inset:0;
  width:100%;
  height:100%;
  touch-action:none;
}

#top{
  position:absolute;
  top:10px;
  left:10px;
  right:10px;
  height:42px;
  display:flex;
  gap:8px;
  z-index:5;
}

.topButton{
  width:45px;
  height:42px;
  border-radius:10px;
  color:white;
  background:#10101dcc;
  border:1px solid #444;
  font-size:18px;
}

#bar{
  flex:1;
  height:10px;
  margin:auto 0;
  border-radius:20px;
  background:#222238;
  overflow:hidden;
}

#barFill{
  width:0%;
  height:100%;
  background:#00ffff;
  box-shadow:0 0 15px #00ffff;
}

#mobileJump{
  position:absolute;
  bottom:25px;
  left:50%;
  transform:translateX(-50%);
  width:110px;
  height:70px;
  border-radius:18px;
  background:#00ffff22;
  border:2px solid #00ffff88;
  color:white;
  font-weight:bold;
  font-size:18px;
  z-index:5;
  display:none;
}

.panel{
  position:absolute;
  inset:0;
  display:none;
  align-items:center;
  justify-content:center;
  background:#000b;
  z-index:20;
}

.box{
  width:min(500px,92%);
  max-height:90vh;
  overflow:auto;
  padding:25px;
  border-radius:20px;
  background:#0c0c20;
  border:1px solid #44446a;
  text-align:center;
}

.box h2{
  margin-top:0;
}

.menuLevels{
  display:grid;
  grid-template-columns:1fr 1fr;
  gap:8px;
}

.levelButton{
  padding:13px 6px;
  border-radius:10px;
  background:#17172d;
  color:white;
  border:1px solid #33334e;
}

.levelButton.unlocked{
  border-color:#00ffff;
}

.levelButton.locked{
  opacity:.3;
}

.actionButton{
  width:100%;
  padding:13px;
  margin-top:9px;
  border-radius:10px;
  background:#17172d;
  color:white;
  border:1px solid #33334e;
  font-weight:bold;
}

.actionButton.primary{
  background:#00ffff;
  color:#020207;
}

@media(max-width:700px){
  #mobileJump{
    display:block;
  }
}
</style>
</head>

<body>

<div id="home">

  <h1 class="logo">NEON DASH</h1>

  <div class="subtitle">
    Jump • Dodge • Collect • Survive
  </div>

  <button class="mainButton" onclick="startGame()">
    ▶ ZAGRAJ
  </button>

  <div class="homeStats">

    <div class="stat">
      <b id="completedCount">0/12</b>
      POZIOMY
    </div>

    <div class="stat">
      <b id="coinCount">0/36</b>
      MONETY
    </div>

    <div class="stat">
      <b>12</b>
      LEVELI
    </div>

  </div>

  <div class="levelList" id="levelList"></div>

</div>


<div id="game">

  <canvas id="canvas"></canvas>

  <div id="top">

    <button class="topButton" onclick="pauseGame()">Ⅱ</button>

    <div id="bar">
      <div id="barFill"></div>
    </div>

    <button class="topButton" onclick="toggleSound()" id="soundButton">
      🔊
    </button>

    <button class="topButton" onclick="showLevels()">
      ☰
    </button>

  </div>

  <button id="mobileJump">
    JUMP
  </button>


  <div class="panel" id="pausePanel">

    <div class="box">

      <h2>PAUZA</h2>

      <button class="actionButton primary"
              onclick="resumeGame()">
        ▶ KONTYNUUJ
      </button>

      <button class="actionButton"
              onclick="restartLevel()">
        ↻ RESTART
      </button>

      <button class="actionButton"
              onclick="showLevels()">
        POZIOMY
      </button>

      <button class="actionButton"
              onclick="backHome()">
        MENU GŁÓWNE
      </button>

    </div>

  </div>


  <div class="panel" id="levelsPanel">

    <div class="box">

      <h2>POZIOMY</h2>

      <div class="menuLevels" id="menuLevels"></div>

      <button class="actionButton"
              onclick="closeLevels()">
        WRÓĆ
      </button>

    </div>

  </div>


  <div class="panel" id="deadPanel">

    <div class="box">

      <h2>💥 KONIEC</h2>

      <p>Spróbuj jeszcze raz!</p>

      <button class="actionButton primary"
              onclick="restartLevel()">
        ↻ SPRÓBUJ PONOWNIE
      </button>

      <button class="actionButton"
              onclick="showLevels()">
        POZIOMY
      </button>

      <button class="actionButton"
              onclick="backHome()">
        MENU GŁÓWNE
      </button>

    </div>

  </div>


  <div class="panel" id="winPanel">

    <div class="box">

      <h2>🎉 POZIOM UKOŃCZONY!</h2>

      <p id="winInfo"></p>

      <button
        class="actionButton primary"
        id="nextLevelButton"
        onclick="nextLevel()">
        NASTĘPNY POZIOM
      </button>

      <button class="actionButton"
              onclick="restartLevel()">
        ↻ ZAGRAJ PONOWNIE
      </button>

      <button class="actionButton"
              onclick="showLevels()">
        POZIOMY
      </button>

      <button class="actionButton"
              onclick="backHome()">
        MENU GŁÓWNE
      </button>

    </div>

  </div>

</div>


<script>

/* =========================
   DANE GRY
========================= */

const levels=[
  ["NEON RUSH","#00ffff",5.8,7000],
  ["CRYSTAL DRIVE","#6fa8ff",6.0,7300],
  ["LAVA PULSE","#ff482e",6.2,7600],
  ["SKY CIRCUIT","#75ff70",6.4,7900],
  ["VIOLET RUN","#c86cff",6.6,8200],
  ["SHADOW FACTORY","#aaaabb",6.8,8500],
  ["CYBER STORM","#00aaff",7.0,8800],
  ["INFERNO CORE","#ff7800",7.2,9000],
  ["ELECTRIC VOID","#eeff00",7.4,9200],
  ["GRAVITY BREAK","#ff35bb",7.6,9400],
  ["OVERDRIVE","#00ff88",7.8,9600],
  ["FINAL CHAOS","#ffffff",8.0,10000]
];

/* =========================
   ZAPIS
========================= */

let completed=
  JSON.parse(localStorage.getItem("nd_completed")||"[]");

let levelCoins=
  JSON.parse(localStorage.getItem("nd_coins")||"[]");

let sound=
  localStorage.getItem("nd_sound")!=="off";

if(!Array.isArray(completed))completed=[];
if(!Array.isArray(levelCoins))levelCoins=[];

function save(){

  localStorage.setItem(
    "nd_completed",
    JSON.stringify(completed)
  );

  localStorage.setItem(
    "nd_coins",
    JSON.stringify(levelCoins)
  );

  localStorage.setItem(
    "nd_sound",
    sound?"on":"off"
  );
}

function completedTotal(){

  let n=0;

  for(let i=0;i<levels.length;i++){
    if(completed[i])n++;
  }

  return n;
}

function coinsTotal(){

  let n=0;

  for(let i=0;i<levels.length;i++){
    n+=Number(levelCoins[i]||0);
  }

  return n;
}

/* =========================
   DOM
========================= */

const home=document.getElementById("home");
const game=document.getElementById("game");
const canvas=document.getElementById("canvas");
const ctx=canvas.getContext("2d");

const pausePanel=document.getElementById("pausePanel");
const levelsPanel=document.getElementById("levelsPanel");
const deadPanel=document.getElementById("deadPanel");
const winPanel=document.getElementById("winPanel");

const levelList=document.getElementById("levelList");
const menuLevels=document.getElementById("menuLevels");

const completedCount=
  document.getElementById("completedCount");

const coinCount=
  document.getElementById("coinCount");

const barFill=
  document.getElementById("barFill");

const soundButton=
  document.getElementById("soundButton");

const mobileJump=
  document.getElementById("mobileJump");

const winInfo=
  document.getElementById("winInfo");

const nextLevelButton=
  document.getElementById("nextLevelButton");

/* =========================
   CANVAS
========================= */

let W=0;
let H=0;
let dpr=1;

function resize(){

  dpr=Math.min(window.devicePixelRatio||1,2);

  W=window.innerWidth;
  H=window.innerHeight;

  canvas.width=W*dpr;
  canvas.height=H*dpr;

  canvas.style.width=W+"px";
  canvas.style.height=H+"px";

  ctx.setTransform(dpr,0,0,dpr,0,0);
}

window.addEventListener("resize",resize);

/* =========================
   GRA
========================= */

let currentLevel=0;

let playing=false;
let paused=false;
let dead=false;
let won=false;

let progress=0;
let lastTime=0;

let obstacles=[];
let coins=[];
let particles=[];
let stars=[];

let collected=0;

/* =========================
   GRACZ
========================= */

const player={
  x:120,
  y:0,
  size:32,
  vy:0,
  grounded:true,
  rotation:0
};

function ground(){
  return H*.78;
}

/* =========================
   GWIAZDY
========================= */

function makeStars(){

  stars=[];

  for(let i=0;i<100;i++){

    stars.push({
      x:Math.random()*W,
      y:Math.random()*H*.7,
      r:.5+Math.random()*1.7,
      speed:.2+Math.random()*.8
    });
  }
}

/* =========================
   POZIOM
========================= */

function createLevel(){

  obstacles=[];
  coins=[];
  particles=[];

  progress=0;
  collected=0;

  const length=levels[currentLevel][3];

  let x=500;

  while(x<length-400){

    const type=Math.random();

    if(type<.58){

      obstacles.push({
        type:"spike",
        x:x,
        w:38,
        h:40
      });

      if(Math.random()<.18){

        obstacles.push({
          type:"spike",
          x:x+42,
          w:38,
          h:40
        });

        x+=42;
      }

    }else if(type<.82){

      obstacles.push({
        type:"saw",
        x:x,
        w:46,
        h:46
      });

    }else{

      obstacles.push({
        type:"gap",
        x:x,
        w:80+Math.random()*30
      });
    }

    x+=280+Math.random()*300;
  }

  /*
    ZAWSZE DOKŁADNIE 3 MONETY
  */

  const positions=[
    length*.27,
    length*.55,
    length*.80
  ];

  for(let i=0;i<3;i++){

    coins.push({
      x:positions[i],
      y:ground()-100-(i===1?65:0),
      collected:false,
      r:11
    });
  }

  player.x=120;
  player.y=ground()-player.size;
  player.vy=0;
  player.grounded=true;
  player.rotation=0;
}

/* =========================
   RYSOWANIE TŁA
========================= */

function drawBackground(){

  const color=levels[currentLevel][1];

  const gradient=ctx.createLinearGradient(
    0,0,0,H
  );

  gradient.addColorStop(0,"#11112d");
  gradient.addColorStop(.65,"#050511");
  gradient.addColorStop(1,"#020207");

  ctx.fillStyle=gradient;
  ctx.fillRect(0,0,W,H);

  for(const s of stars){

    let x=
      (s.x-progress*s.speed*.08)%
      (W+30);

    if(x<0)x+=W+30;

    ctx.fillStyle="#ffffff";
    ctx.globalAlpha=.2+s.speed*.4;

    ctx.beginPath();
    ctx.arc(x,s.y,s.r,0,Math.PI*2);
    ctx.fill();
  }

  ctx.globalAlpha=1;

  /*
    neonowa siatka
  */

  ctx.strokeStyle=color+"25";
  ctx.lineWidth=1;

  const g=ground();

  for(let x=0;x<W;x+=50){

    ctx.beginPath();
    ctx.moveTo(x,g);
    ctx.lineTo(
      W/2+(x-W/2)*.2,
      H
    );
    ctx.stroke();
  }

  for(let y=g;y<H;y+=35){

    ctx.beginPath();
    ctx.moveTo(0,y);
    ctx.lineTo(W,y);
    ctx.stroke();
  }
}

/* =========================
   TEREN
========================= */

function drawGround(){

  const color=levels[currentLevel][1];
  const g=ground();

  ctx.fillStyle="#080812";
  ctx.fillRect(0,g,W,H-g);

  ctx.fillStyle=color;

  ctx.shadowBlur=15;
  ctx.shadowColor=color;

  ctx.fillRect(0,g,W,3);

  ctx.shadowBlur=0;
}

/* =========================
   GRACZ
========================= */

function drawPlayer(){

  const color=levels[currentLevel][1];

  ctx.save();

  ctx.translate(
    player.x+player.size/2,
    player.y+player.size/2
  );

  ctx.rotate(player.rotation);

  ctx.shadowBlur=18;
  ctx.shadowColor=color;

  ctx.fillStyle="#ffffff";

  ctx.fillRect(
    -player.size/2,
    -player.size/2,
    player.size,
    player.size
  );

  ctx.strokeStyle=color;
  ctx.lineWidth=3;

  ctx.strokeRect(
    -player.size/2,
    -player.size/2,
    player.size,
    player.size
  );

  ctx.restore();
}

/* =========================
   OBSTAKLE
========================= */

function drawObstacle(o){

  const color=levels[currentLevel][1];

  const x=o.x-progress;

  if(x<-100||x>W+100)return;

  ctx.save();

  if(o.type==="spike"){

    ctx.fillStyle=color;

    ctx.shadowBlur=14;
    ctx.shadowColor=color;

    ctx.beginPath();

    ctx.moveTo(x,ground());
    ctx.lineTo(
      x+o.w/2,
      ground()-o.h
    );
    ctx.lineTo(
      x+o.w,
      ground()
    );

    ctx.closePath();
    ctx.fill();

  }else if(o.type==="saw"){

    const cx=x+o.w/2;
    const cy=ground()-o.h/2;

    ctx.translate(cx,cy);
    ctx.rotate(progress*.01);

    ctx.fillStyle=color;

    ctx.shadowBlur=15;
    ctx.shadowColor=color;

    const teeth=12;

    ctx.beginPath();

    for(let i=0;i<teeth*2;i++){

      const a=
        i*Math.PI/teeth;

      const r=
        i%2===0
        ? o.w/2
        : o.w*.34;

      const px=Math.cos(a)*r;
      const py=Math.sin(a)*r;

      if(i===0)
        ctx.moveTo(px,py);
      else
        ctx.lineTo(px,py);
    }

    ctx.closePath();
    ctx.fill();

    ctx.fillStyle="#05050b";

    ctx.beginPath();
    ctx.arc(0,0,7,0,Math.PI*2);
    ctx.fill();

  }else if(o.type==="gap"){

    ctx.fillStyle="#000";

    ctx.fillRect(
      x,
      ground(),
      o.w,
      H-ground()
    );

    ctx.strokeStyle=color;
    ctx.lineWidth=2;

    ctx.strokeRect(
      x,
      ground(),
      o.w,
      H-ground()
    );
  }

  ctx.restore();
}

/* =========================
   MONETY
========================= */

function drawCoins(){

  for(const c of coins){

    if(c.collected)continue;

    const x=c.x-progress;

    if(x<-30||x>W+30)continue;

    ctx.save();

    ctx.translate(x,c.y);

    ctx.rotate(Math.sin(progress*.02)*.2);

    ctx.fillStyle="#ffd900";
    ctx.shadowBlur=18;
    ctx.shadowColor="#ffd900";

    ctx.beginPath();
    ctx.arc(0,0,c.r,0,Math.PI*2);
    ctx.fill();

    ctx.fillStyle="#fff4a0";

    ctx.beginPath();
    ctx.arc(-3,-3,3,0,Math.PI*2);
    ctx.fill();

    ctx.restore();
  }
}

/* =========================
   CZĄSTECZKI
========================= */

function spawnParticles(x,y,color,count){

  for(let i=0;i<count;i++){

    particles.push({
      x:x,
      y:y,
      vx:(Math.random()-.5)*6,
      vy:(Math.random()-.5)*6,
      life:25+Math.random()*30,
      size:2+Math.random()*3,
      color:color
    });
  }
}

function updateParticles(dt){

  for(let i=particles.length-1;i>=0;i--){

    const p=particles[i];

    p.x+=p.vx*dt;
    p.y+=p.vy*dt;

    p.life-=dt;

    if(p.life<=0){
      particles.splice(i,1);
    }
  }
}

function drawParticles(){

  for(const p of particles){

    ctx.globalAlpha=
      Math.max(0,p.life/50);

    ctx.fillStyle=p.color;

    ctx.fillRect(
      p.x,
      p.y,
      p.size,
      p.size
    );
  }

  ctx.globalAlpha=1;
}

/* =========================
   SKOK
========================= */

function jump(){

  if(!playing)return;
  if(paused)return;
  if(dead)return;
  if(won)return;

  /*
    WAŻNE:
    skok tylko po kliknięciu / klawiszu
  */

  if(player.grounded){

    player.vy=-12;
    player.grounded=false;

    spawnParticles(
      player.x+player.size/2,
      player.y+player.size,
      levels[currentLevel][1],
      8
    );

    soundJump();
  }
}

/* =========================
   AUDIO
========================= */

let audio=null;

function initAudio(){

  if(audio)return;

  try{
    audio=
      new AudioContext();
  }catch(e){}
}

function beep(freq,duration){

  if(!sound)return;

  initAudio();

  if(!audio)return;

  try{

    const osc=
      audio.createOscillator();

    const gain=
      audio.createGain();

    osc.frequency.value=freq;
    osc.type="square";

    gain.gain.value=.035;

    osc.connect(gain);
    gain.connect(audio.destination);

    osc.start();

    gain.gain.exponentialRampToValueAtTime(
      .001,
      audio.currentTime+duration
    );

    osc.stop(
      audio.currentTime+duration
    );

  }catch(e){}
}

function soundJump(){
  beep(650,.06);
}

function soundCoin(){
  beep(900,.08);
}

function soundDeath(){
  beep(120,.18);
}

/* =========================
   KOLIZJE
========================= */

function collisionRect(a,b){

  return(
    a.x<b.x+b.w &&
    a.x+a.w>b.x &&
    a.y<b.y+b.h &&
    a.y+a.h>b.y
  );
}

function updateCollisions(){

  const p={
    x:player.x+5,
    y:player.y+5,
    w:player.size-10,
    h:player.size-10
  };

  for(const o of obstacles){

    const x=o.x-progress;

    if(o.type==="spike"){

      const hit=
        p.x<x+o.w &&
        p.x+p.w>x &&
        p.y+p.h>ground()-o.h+8;

      if(hit){
        die();
        return;
      }

    }else if(o.type==="saw"){

      const cx=x+o.w/2;
      const cy=ground()-o.h/2;

      const closestX=
        Math.max(p.x,Math.min(cx,p.x+p.w));

      const closestY=
        Math.max(p.y,Math.min(cy,p.y+p.h));

      const dx=cx-closestX;
      const dy=cy-closestY;

      if(
        dx*dx+dy*dy<
        (o.w*.43)*(o.w*.43)
      ){
        die();
        return;
      }

    }else if(o.type==="gap"){

      if(
        player.grounded &&
        p.x<x+o.w &&
        p.x+p.w>x
      ){
        die();
        return;
      }
    }
  }

  for(const c of coins){

    if(c.collected)continue;

    const x=c.x-progress;

    const dx=
      player.x+player.size/2-x;

    const dy=
      player.y+player.size/2-c.y;

    if(
      dx*dx+dy*dy<
      (c.r+player.size*.45)*
      (c.r+player.size*.45)
    ){

      c.collected=true;
      collected++;

      spawnParticles(
        x,
        c.y,
        "#ffd900",
        15
      );

      soundCoin();
    }
  }
}

/* =========================
   ŚMIERĆ
========================= */

function die(){

  if(dead)return;

  dead=true;
  playing=false;

  spawnParticles(
    player.x+player.size/2,
    player.y+player.size/2,
    levels[currentLevel][1],
    35
  );

  soundDeath();

  deadPanel.style.display="flex";
}

/* =========================
   WYGRANA
========================= */

function win(){

  if(won)return;

  won=true;
  playing=false;

  completed[currentLevel]=true;

  const old=Number(levelCoins[currentLevel]||0);

  levelCoins[currentLevel]=
    Math.max(old,collected);

  save();

  const total=
    Number(levelCoins[currentLevel]||0);

  winInfo.textContent=
    "Poziom: "+
    levels[currentLevel][0]+
    " • Monety: "+
    total+"/3";

  if(currentLevel===levels.length-1){

    nextLevelButton.style.display="none";

  }else{

    nextLevelButton.style.display="block";
    nextLevelButton.textContent=
      "NASTĘPNY POZIOM";
  }

  winPanel.style.display="flex";

  updateHome();
}

/* =========================
   UPDATE
========================= */

function update(dt){

  if(!playing)return;
  if(paused)return;
  if(dead)return;
  if(won)return;

  const speed=levels[currentLevel][2];
  const length=levels[currentLevel][3];

  progress+=speed*dt;

  if(progress>=length){

    progress=length;

    win();

    return;
  }

  /*
    fizyka gracza
  */

  player.vy+=.72*dt;

  player.y+=player.vy*dt;

  if(
    player.y+player.size>=ground()
  ){

    player.y=
      ground()-player.size;

    player.vy=0;
    player.grounded=true;

    /*
      NIE ustawiamy żadnego automatycznego
      skoku tutaj.
    */

  }else{

    player.grounded=false;
  }

  if(!player.grounded){

    player.rotation+=.08*dt;

  }else{

    player.rotation=
      Math.round(
        player.rotation/(Math.PI/2)
      )*(Math.PI/2);
  }

  updateCollisions();
  updateParticles(dt);

  barFill.style.width=
    (progress/length*100)+"%";
}

/* =========================
   DRAW
========================= */

function draw(){

  ctx.clearRect(0,0,W,H);

  drawBackground();
  drawGround();

  for(const o of obstacles){
    drawObstacle(o);
  }

  drawCoins();
  drawParticles();

  if(!dead){
    drawPlayer();
  }
}

/* =========================
   LOOP
========================= */

function loop(time){

  if(!lastTime)
    lastTime=time;

  let dt=
    (time-lastTime)/16.6667;

  lastTime=time;

  dt=Math.min(dt,2);

  update(dt);
  draw();

  requestAnimationFrame(loop);
}

requestAnimationFrame(loop);

/* =========================
   START
========================= */

function startGame(){

  home.style.display="none";
  game.style.display="block";

  const firstLocked=
    completedTotal();

  let level=firstLocked;

  if(level>=levels.length)
    level=levels.length-1;

  startLevel(level);
}

function startLevel(index){

  if(index<0||index>=levels.length)
    return;

  if(
    index>0 &&
    !completed[index-1]
  ){

    return;
  }

  currentLevel=index;

  pausePanel.style.display="none";
  levelsPanel.style.display="none";
  deadPanel.style.display="none";
  winPanel.style.display="none";

  paused=false;
  dead=false;
  won=false;
  playing=true;

  createLevel();

  lastTime=performance.now();

  initAudio();
}

/* =========================
   RESTART
========================= */

function restartLevel(){

  pausePanel.style.display="none";
  deadPanel.style.display="none";
  winPanel.style.display="none";
  levelsPanel.style.display="none";

  paused=false;
  dead=false;
  won=false;
  playing=true;

  createLevel();

  lastTime=performance.now();
}

/* =========================
   PAUZA
========================= */

function pauseGame(){

  if(!playing)return;

  playing=false;
  paused=true;

  pausePanel.style.display="flex";
}

function resumeGame(){

  pausePanel.style.display="none";

  if(!dead&&!won){

    paused=false;
    playing=true;
    lastTime=performance.now();
  }
}

/* =========================
   POZIOMY
========================= */

function showLevels(){

  pausePanel.style.display="none";
  winPanel.style.display="none";
  deadPanel.style.display="none";

  playing=false;
  paused=true;

  buildMenuLevels();

  levelsPanel.style.display="flex";
}

function closeLevels(){

  levelsPanel.style.display="none";

  if(!dead&&!won){

    paused=false;
    playing=true;

    lastTime=performance.now();
  }
}

function buildMenuLevels(){

  menuLevels.innerHTML="";

  for(let i=0;i<levels.length;i++){

    const unlocked=
      i===0 ||
      completed[i-1];

    const button=
      document.createElement("button");

    button.className=
      "levelButton "+
      (unlocked?"unlocked":"locked");

    button.innerHTML=
      (i+1)+". "+levels[i][0]+
      "<br><small>"+
      (
        unlocked
        ? "🪙 "+Number(levelCoins[i]||0)+"/3"
        : "🔒 ZABLOKOWANY"
      )+
      "</small>";

    if(unlocked){

      button.onclick=()=>{
        startLevel(i);
      };

    }else{

      button.onclick=()=>{
        alert(
          "Najpierw ukończ poprzedni poziom."
        );
      };
    }

    menuLevels.appendChild(button);
  }
}

/* =========================
   NASTĘPNY
========================= */

function nextLevel(){

  if(currentLevel<levels.length-1){

    startLevel(currentLevel+1);

  }else{

    backHome();
  }
}

/* =========================
   MENU GŁÓWNE
========================= */

function backHome(){

  playing=false;
  paused=false;
  dead=false;
  won=false;

  game.style.display="none";
  home.style.display="block";

  pausePanel.style.display="none";
  levelsPanel.style.display="none";
  deadPanel.style.display="none";
  winPanel.style.display="none";

  updateHome();
}

/* =========================
   DŹWIĘK
========================= */

function toggleSound(){

  sound=!sound;

  soundButton.textContent=
    sound?"🔊":"🔇";

  save();

  if(sound){
    beep(700,.06);
  }
}

/* =========================
   KLAWISZE
========================= */

window.addEventListener("keydown",function(e){

  if(
    e.code==="Space" ||
    e.code==="ArrowUp" ||
    e.code==="KeyW"
  ){

    e.preventDefault();

    if(!e.repeat){
      jump();
    }
  }

  if(e.code==="Escape"){

    if(
      game.style.display==="block" &&
      playing
    ){
      pauseGame();
    }
  }

  if(
    e.code==="KeyR" &&
    game.style.display==="block"
  ){
    restartLevel();
  }
});

/* =========================
   MYSZ
========================= */

canvas.addEventListener(
  "pointerdown",
  function(e){

    /*
      kliknięcie w grę = skok
    */

    if(
      e.pointerType==="mouse" ||
      e.pointerType==="pen"
    ){

      jump();
    }
  }
);

/* =========================
   TELEFON
========================= */

mobileJump.addEventListener(
  "pointerdown",
  function(e){

    e.preventDefault();

    jump();
  }
);

/*
  Dotknięcie dowolnego miejsca canvasem
  również działa na telefonie.
*/

canvas.addEventListener(
  "touchstart",
  function(e){

    e.preventDefault();

    jump();
  },
  {passive:false}
);

/* =========================
   AUTO PAUZA
========================= */

document.addEventListener(
  "visibilitychange",
  function(){

    if(
      document.hidden &&
      playing &&
      !dead &&
      !won
    ){

      pauseGame();
    }
  }
);

/* =========================
   STRONA GŁÓWNA
========================= */

function updateHome(){

  completedCount.textContent=
    completedTotal()+"/12";

  coinCount.textContent=
    coinsTotal()+"/36";

  levelList.innerHTML="";

  for(let i=0;i<levels.length;i++){

    const unlocked=
      i===0 ||
      completed[i-1];

    const card=
      document.createElement("div");

    card.className=
      "levelCard "+
      (unlocked?"":"locked");

    card.style.borderColor=
      unlocked
      ? levels[i][1]+"77"
      : "";

    card.innerHTML=
      "<div style='color:"+
      levels[i][1]+
      "'>LEVEL "+(i+1)+"</div>"+
      "<div class='levelName'>"+
      levels[i][0]+
      "</div>"+
      "<div class='levelInfo'>"+
      (
        unlocked
        ? "🪙 "+Number(levelCoins[i]||0)+"/3"
        : "🔒 ZABLOKOWANY"
      )+
      "</div>";

    levelList.appendChild(card);
  }
}

/* =========================
   START
========================= */

resize();
makeStars();

soundButton.textContent=
  sound?"🔊":"🔇";

updateHome();

save();

</script>

</body>
</html>
```
