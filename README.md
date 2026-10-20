<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
<meta name="theme-color" content="#07111f">
<title>AHC VIP GAME — Garage VIP Racing</title>

<style>
*{box-sizing:border-box}
:root{
 --bg:#07111f;
 --panel:#0d2138;
 --cyan:#39e5f2;
 --gold:#ffc857;
 --text:#f4f8ff;
 --muted:#a9bbce;
}
body{
 margin:0;background:var(--bg);color:var(--text);
 font-family:Arial,Helvetica,sans-serif;
}
header{
 padding:20px 12px;text-align:center;
 background:linear-gradient(135deg,#0b1c31,#102d4b);
 border-bottom:2px solid var(--cyan);
}
header h1{margin:0;color:var(--cyan);font-size:27px}
header p{margin:8px 0 0;color:var(--muted);font-size:13px}
main{width:95%;max-width:760px;margin:16px auto}
.card{
 background:var(--panel);border:1px solid #244765;
 border-radius:17px;padding:16px;margin-bottom:16px;
 box-shadow:0 7px 20px #0004;
}
h2{margin-top:0;color:var(--cyan);font-size:20px}
h3{color:var(--gold)}
p{line-height:1.5}
button,select{
 font-size:15px;border-radius:10px;padding:12px;
 border:1px solid #315675;
}
button{
 background:#087eaa;color:white;font-weight:bold;
 cursor:pointer;touch-action:manipulation;
}
button:active{transform:scale(.98)}
button.secondary{background:#253d56}
button.gold{background:#a66b09;color:#fff}
button.red{background:#8d2935}
select{
 width:100%;background:#071827;color:white;
 margin-top:5px;
}
label{display:block;margin:12px 0 5px;color:#c8d6e5}
.small{font-size:12px;color:var(--muted)}
.stats{
 display:grid;grid-template-columns:repeat(3,1fr);
 gap:8px;margin-bottom:12px;
}
.stat{
 text-align:center;background:#071827;
 border:1px solid #244765;border-radius:12px;padding:12px 5px;
}
.stat strong{display:block;color:var(--gold);font-size:20px}
.stat span{font-size:11px;color:var(--muted)}
.row{display:flex;gap:8px;flex-wrap:wrap}
.row>*{flex:1;min-width:100px}
.message{
 padding:11px;background:#071827;border-radius:10px;
 margin-top:10px;line-height:1.5;
}
.hidden{display:none!important}
#raceCanvas{
 display:block;width:100%;height:auto;max-width:420px;
 margin:12px auto;background:#172d2a;
 border:2px solid #345a72;border-radius:12px;
 touch-action:none;
}
.garage-preview{
 display:grid;grid-template-columns:1fr 1fr;
 gap:12px;align-items:center;
}
.car-preview{
 min-height:110px;border:1px solid #315675;
 background:#071827;border-radius:12px;
 display:flex;align-items:center;justify-content:center;
 flex-direction:column;text-align:center;padding:10px;
}
.car-icon{font-size:44px}
.car-name{color:var(--gold);font-weight:bold;margin-top:5px}
.race-controls{
 display:grid;grid-template-columns:repeat(3,1fr);
 gap:8px;margin-top:12px;
}
.race-controls button{min-height:58px;padding:9px 4px}
.race-controls .wide{grid-column:span 3}
.mode-button{min-height:48px}
.active-mode{border:2px solid var(--gold);background:#164b63}
.shop-item{
 padding:12px;border:1px solid #244765;
 border-radius:10px;margin:8px 0;background:#071827;
}
footer{text-align:center;padding:18px;color:var(--muted);font-size:12px}
@media(max-width:400px){
 header h1{font-size:23px}
 .card{padding:12px}
 .stat strong{font-size:17px}
 .race-controls button{font-size:13px}
}
</style>
</head>

<body>
<header>
 <h1>🏆 AHC VIP GAME</h1>
 <p>Quiz • Réflexes • Garage VIP • Racing</p>
</header>

<main>

<section class="card">
 <h2>🎮 Tableau de bord</h2>
 <div class="stats">
  <div class="stat"><strong id="points">0</strong><span>POINTS VIP</span></div>
  <div class="stat"><strong id="level">1</strong><span>NIVEAU</span></div>
  <div class="stat"><strong id="bestScore">0</strong><span>MEILLEUR SCORE</span></div>
 </div>
 <p class="small">Tes points et ton meilleur score sont conservés sur cet appareil grâce au stockage local du navigateur.</p>
</section>

<section class="card" id="quizSection">
 <h2>🧠 Quiz VIP</h2>
 <p id="quizQuestion">Quelle est la capitale d’Haïti ?</p>
 <div id="quizAnswers" class="row">
  <button onclick="answerQuiz(0)">Cap-Haïtien</button>
  <button onclick="answerQuiz(1)">Port-au-Prince</button>
  <button onclick="answerQuiz(2)">Les Cayes</button>
 </div>
 <div id="quizMessage" class="message">Choisis une réponse pour gagner des points.</div>
 <button class="secondary" onclick="nextQuiz()">Question suivante</button>
</section>

<section class="card" id="reflexSection">
 <h2>⚡ Jeu de réflexes</h2>
 <p>Appuie sur le bouton dès qu’il devient vert. Le but est de réagir rapidement.</p>
 <button id="reflexButton" class="red" onclick="reflexAction()" style="min-height:90px;width:100%">
  APPUIE POUR COMMENCER
 </button>
 <div id="reflexMessage" class="message">Prêt à tester tes réflexes ?</div>
</section>

<section class="card" id="garageSection">
 <h2>🚗 Garage VIP personnalisé</h2>
 <p>Choisis une marque, un modèle et une couleur. La sélection sera utilisée dans les deux circuits.</p>

 <label for="carBrand">Marque</label>
 <select id="carBrand" onchange="updateGarage()">
  <option value="Ferrari">Ferrari</option>
  <option value="Lamborghini">Lamborghini</option>
  <option value="Porsche">Porsche</option>
  <option value="Bugatti">Bugatti</option>
 </select>

 <label for="carModel">Modèle</label>
 <select id="carModel" onchange="updateGarage()">
  <option value="F8">F8 — Sportive</option>
  <option value="SF90">SF90 — Hybride sportive</option>
 </select>

 <label for="carColor">Couleur de carrosserie</label>
 <select id="carColor" onchange="updateGarage()">
  <option value="#ff3030">Rouge racing</option>
  <option value="#39e5f2">Cyan électrique</option>
  <option value="#f5f5f5">Blanc nacré</option>
  <option value="#11151c">Noir brillant</option>
  <option value="#ffc857">Or VIP</option>
  <option value="#16c784">Vert compétition</option>
  <option value="#376dff">Bleu sport</option>
 </select>

 <div class="garage-preview" style="margin-top:14px">
  <div class="car-preview">
   <div class="car-icon" id="garageIcon">🏎️</div>
   <div class="car-name" id="garageName">Ferrari F8</div>
   <div class="small" id="garageColorText">Rouge racing</div>
  </div>
  <div>
   <p><strong>⚙️ Performances</strong></p>
   <p class="small">Accélération : <strong id="statAccel">8/10</strong></p>
   <p class="small">Vitesse : <strong id="statSpeed">9/10</strong></p>
   <p class="small">Maniabilité : <strong id="statHandling">8/10</strong></p>
  </div>
 </div>
 <div id="garageMessage" class="message">Ta voiture personnalisée est prête.</div>
</section>

<section class="card" id="racingSection">
 <h2>🏁 AHC VIP RACING</h2>

 <div class="row">
  <button id="topMode" class="mode-button active-mode" onclick="setMode('top')">
   🛣️ Vue de dessus
  </button>
  <button id="rearMode" class="mode-button" onclick="setMode('rear')">
   🏎️ Perspective arrière
  </button>
 </div>

 <div class="stats" style="margin-top:12px">
  <div class="stat"><strong id="raceScore">0</strong><span>SCORE COURSE</span></div>
  <div class="stat"><strong id="raceLevel">1</strong><span>NIVEAU</span></div>
  <div class="stat"><strong id="raceSpeed">0</strong><span>VITESSE</span></div>
 </div>

 <canvas id="raceCanvas" width="360" height="560">
  Votre navigateur ne prend pas en charge le canvas.
 </canvas>

 <div class="race-controls">
  <button class="secondary" onclick="moveCar(-1)">⬅️ GAUCHE</button>
  <button class="secondary" onclick="moveCar(1)">DROITE ➡️</button>
  <button class="gold" id="startButton" onclick="startRace()">▶️ DÉPART</button>

  <button id="accelerateButton">⚡ ACCÉLÉRER</button>
  <button class="gold" id="turboButton">🚀 TURBO</button>
  <button class="secondary" id="brakeButton">🛑 FREINER</button>

  <button class="red wide" onclick="closeRace()">⏹️ ARRÊTER LA COURSE</button>
 </div>

 <div id="raceMessage" class="message">
  Sélectionne ta voiture, choisis une vue puis appuie sur DÉPART.
 </div>

 <p class="small">
  Commandes tactiles : utilise les boutons pour changer de voie,
  accélérer, activer le turbo et freiner. Tu peux aussi utiliser les
  flèches du clavier sur un ordinateur.
 </p>
</section>

<section class="card" id="shopSection">
 <h2>🛒 Boutique virtuelle VIP</h2>
 <p>Les achats sont uniquement simulés dans le jeu. Aucun paiement réel n’est effectué.</p>
 <div class="shop-item">
  <strong>🎨 Peinture VIP — 100 points</strong>
  <p class="small">Débloque une personnalisation virtuelle de ton garage.</p>
  <button onclick="buyItem('paint',100)">Acheter — 100 points</button>
 </div>
 <div class="shop-item">
  <strong>⚡ Boost de course — 150 points</strong>
  <p class="small">Débloque un bonus de turbo pour les prochaines courses.</p>
  <button onclick="buyItem('boost',150)">Acheter — 150 points</button>
 </div>
 <div class="shop-item">
  <strong>🏆 Pack VIP — 250 points</strong>
  <p class="small">Ajoute un bonus virtuel de points au compte du jeu.</p>
  <button onclick="buyItem('vip',250)">Acheter — 250 points</button>
 </div>
 <div id="shopMessage" class="message">Tes achats virtuels seront enregistrés sur cet appareil.</div>
</section>

</main>

<footer>
 AHС VIP GAME © 2026 — Jeu de démonstration local
</footer>

<script>
"use strict";

/* ======================================================
   STOCKAGE LOCAL ET POINTS
====================================================== */

const STORAGE_KEY = "AHC_VIP_GAME_UPDATEPRO_V2";

let savedData = {};
try {
 savedData = JSON.parse(localStorage.getItem(STORAGE_KEY) || "{}") || {};
} catch(e) {
 savedData = {};
}

let points = Number(savedData.points) || 0;
let totalLevel = Number(savedData.level) || 1;
let bestScore = Number(savedData.bestScore) || 0;
let ownedItems = Array.isArray(savedData.ownedItems) ? savedData.ownedItems : [];
let purchasedBoost = Boolean(savedData.purchasedBoost);
let quizIndex = 0;
let quizAnswered = false;

function saveGame(){
 try {
  localStorage.setItem(STORAGE_KEY, JSON.stringify({
   points:points,
   level:totalLevel,
   bestScore:bestScore,
   ownedItems:ownedItems,
   purchasedBoost:purchasedBoost
  }));
 } catch(e) {
  console.warn("Le stockage local n’est pas disponible.");
 }
 updateDashboard();
}

function updateDashboard(){
 document.getElementById("points").textContent = points;
 document.getElementById("level").textContent = totalLevel;
 document.getElementById("bestScore").textContent = bestScore;
}

function earnPoints(amount){
 points += amount;
 totalLevel = Math.max(1, Math.floor(points / 100) + 1);
 saveGame();
}

/* ======================================================
   QUIZ
====================================================== */

const quizData = [
 {
  q:"Quelle est la capitale d’Haïti ?",
  a:["Cap-Haïtien","Port-au-Prince","Les Cayes"],
  correct:1
 },
 {
  q:"Combien de côtés possède un triangle ?",
  a:["Trois","Quatre","Cinq"],
  correct:0
 },
 {
  q:"Quelle planète est connue comme la planète rouge ?",
  a:["Vénus","Mars","Jupiter"],
  correct:1
 },
 {
  q:"Combien font 12 × 5 ?",
  a:["50","55","60"],
  correct:2
 },
 {
  q:"Quel organe pompe le sang dans le corps humain ?",
  a:["Le cœur","Le foie","Le poumon"],
  correct:0
 }
];

function renderQuiz(){
 const q = quizData[quizIndex % quizData.length];
 document.getElementById("quizQuestion").textContent = q.q;

 const box = document.getElementById("quizAnswers");
 box.innerHTML = "";

 q.a.forEach((answer,index)=>{
  const btn = document.createElement("button");
  btn.textContent = answer;
  btn.onclick = ()=>answerQuiz(index);
  box.appendChild(btn);
 });

 quizAnswered = false;
 document.getElementById("quizMessage").textContent =
  "Choisis une réponse pour gagner des points.";
}

function answerQuiz(index){
 if(quizAnswered) return;

 quizAnswered = true;
 const q = quizData[quizIndex % quizData.length];
 const buttons = document.querySelectorAll("#quizAnswers button");

 if(index === q.correct){
  earnPoints(10);
  document.getElementById("quizMessage").textContent =
   "✅ Bonne réponse ! +10 points VIP.";
  buttons[index].style.background = "#137b52";
 } else {
  document.getElementById("quizMessage").textContent =
   "❌ Pas tout à fait. La bonne réponse était : " + q.a[q.correct] + ".";
  if(buttons[index]) buttons[index].style.background = "#8d2935";
  if(buttons[q.correct]) buttons[q.correct].style.background = "#137b52";
 }
}

function nextQuiz(){
 quizIndex++;
 renderQuiz();
}

/* ======================================================
   JEU DE RÉFLEXES
====================================================== */

let reflexState = "idle";
let reflexStart = 0;
let reflexTimer = null;

function reflexAction(){
 const btn = document.getElementById("reflexButton");
 const msg = document.getElementById("reflexMessage");

 if(reflexState === "idle" || reflexState === "done"){
  reflexState = "waiting";
  btn.className = "red";
  btn.textContent = "ATTENDS LE SIGNAL...";
  msg.textContent = "Ne touche pas avant que le bouton devienne vert.";

  clearTimeout(reflexTimer);
  reflexTimer = setTimeout(()=>{
   if(reflexState !== "waiting") return;
   reflexState = "go";
   reflexStart = performance.now();
   btn.className = "";
   btn.style.background = "#10a65b";
   btn.textContent = "MAINTENANT ! APPUIE !";
   msg.textContent = "Vite !";
  }, 1200 + Math.random()*2800);
  return;
 }

 if(reflexState === "waiting"){
  clearTimeout(reflexTimer);
  reflexState = "done";
  btn.style.background = "#8d2935";
  btn.textContent = "RECOMMENCER";
  msg.textContent = "Trop tôt ! Attends le signal vert avant d’appuyer.";
  return;
 }

 if(reflexState === "go"){
  const reaction = Math.round(performance.now()-reflexStart);
  reflexState = "done";
  btn.style.background = "#087eaa";
  btn.textContent = "RECOMMENCER";

  if(reaction < 350){
   earnPoints(20);
   msg.textContent = "⚡ Excellent ! " + reaction + " ms. +20 points VIP.";
  } else if(reaction < 600){
   earnPoints(10);
   msg.textContent = "👍 Bien joué ! " + reaction + " ms. +10 points VIP.";
  } else {
   msg.textContent = "⏱️ Temps de réaction : " + reaction + " ms. Réessaie pour améliorer ton score.";
  }
 }
}

/* ======================================================
   GARAGE ET PERFORMANCES
====================================================== */

const carCatalog = {
 Ferrari: {
  models:[
   {value:"F8",label:"F8 — Sportive",accel:8,speed:9,handling:8,shape:"ferrari"},
   {value:"SF90",label:"SF90 — Hybride sportive",accel:10,speed:9,handling:8,shape:"ferrari"}
  ]
 },
 Lamborghini: {
  models:[
   {value:"Huracan",label:"Huracán — Supercar",accel:9,speed:9,handling:7,shape:"lamborghini"},
   {value:"Aventador",label:"Aventador — V12",accel:8,speed:10,handling:6,shape:"lamborghini"}
  ]
 },
 Porsche: {
  models:[
   {value:"911",label:"911 — Carrera",accel:8,speed:8,handling:10,shape:"porsche"},
   {value:"GT3",label:"911 GT3 — Piste",accel:9,speed:9,handling:10,shape:"porsche"}
  ]
 },
 Bugatti: {
  models:[
   {value:"Chiron",label:"Chiron — Hypercar",accel:9,speed:10,handling:6,shape:"bugatti"},
   {value:"Tourbillon",label:"Tourbillon — Hypercar",accel:10,speed:10,handling:7,shape:"bugatti"}
  ]
 }
};

const colorNames = {
 "#ff3030":"Rouge racing",
 "#39e5f2":"Cyan électrique",
 "#f5f5f5":"Blanc nacré",
 "#11151c":"Noir brillant",
 "#ffc857":"Or VIP",
 "#16c784":"Vert compétition",
 "#376dff":"Bleu sport"
};

function getSelectedCar(){
 const brand = document.getElementById("carBrand").value;
 const modelValue = document.getElementById("carModel").value;
 const models = carCatalog[brand].models;
 return {
  brand:brand,
  ...models.find(m=>m.value===modelValue) || models[0],
  color:document.getElementById("carColor").value
 };
}

function updateModelOptions(){
 const brand = document.getElementById("carBrand").value;
 const select = document.getElementById("carModel");
 const oldValue = select.value;

 select.innerHTML = "";
 carCatalog[brand].models.forEach(model=>{
  const option = document.createElement("option");
  option.value = model.value;
  option.textContent = model.label;
  select.appendChild(option);
 });

 if(carCatalog[brand].models.some(m=>m.value===oldValue)){
  select.value = oldValue;
 }
}

function updateGarage(){
 if(!carCatalog[document.getElementById("carBrand").value].models.some(
  m=>m.value===document.getElementById("carModel").value
 )){
  updateModelOptions();
 }

 const car = getSelectedCar();
 document.getElementById("garageName").textContent =
  car.brand + " " + car.value;
 document.getElementById("garageColorText").textContent =
  colorNames[car.color] || "Couleur personnalisée";

 document.getElementById("statAccel").textContent = car.accel + "/10";
 document.getElementById("statSpeed").textContent = car.speed + "/10";
 document.getElementById("statHandling").textContent = car.handling + "/10";

 document.getElementById("garageIcon").textContent =
  car.shape === "porsche" ? "🏎️" :
  car.shape === "bugatti" ? "🚘" : "🏎️";

 document.getElementById("garageMessage").textContent =
  "✅ " + car.brand + " " + car.value +
  " est sélectionnée. Couleur et performances appliquées aux deux circuits.";

 drawScene();
}

/* ======================================================
   MOTEUR DE COURSE
====================================================== */

const canvas = document.getElementById("raceCanvas");
const ctx = canvas.getContext("2d");

const W = canvas.width;
const H = canvas.height;

let mode = "top";
let running = false;
let animation = 0;
let lastFrame = 0;
let raceScore = 0;
let raceLevel = 1;
let playerLane = 1;
let playerX = W/2;
let obstacles = [];
let roadOffset = 0;
let currentSpeed = 0;
let accelerating = false;
let braking = false;
let turboTime = 0;
let crashFlash = 0;
let lastObstacleTime = 0;

const laneCount = 3;

function setMode(newMode){
 mode = newMode;

 document.getElementById("topMode").classList.toggle(
  "active-mode",mode==="top"
 );
 document.getElementById("rearMode").classList.toggle(
  "active-mode",mode==="rear"
 );

 drawScene();
 document.getElementById("raceMessage").textContent =
  mode === "top"
   ? "Vue de dessus sélectionnée. La voiture personnalisée est prête."
   : "Perspective arrière sélectionnée. La voiture personnalisée est prête.";
}

function laneCenter(lane){
 return W * (lane + 0.5) / laneCount;
}

/* ======================================================
   OUTILS DE DESSIN
====================================================== */

function roundedRect(x,y,w,h,r,fill,stroke){
 ctx.beginPath();
 ctx.roundRect(x,y,w,h,r);
 if(fill){ctx.fillStyle=fill;ctx.fill();}
 if(stroke){ctx.strokeStyle=stroke;ctx.stroke();}
}

function drawWheel(x,y,w,h){
 ctx.fillStyle = "#05070a";
 roundedRect(x,y,w,h,Math.min(w,h)*0.35,"#05070a","#444b54");
 ctx.fillStyle = "#66717d";
 roundedRect(x+w*0.25,y+h*0.2,w*0.5,h*0.6,2,"#66717d");
}

function polygon(points,fill,stroke){
 ctx.beginPath();
 ctx.moveTo(points[0][0],points[0][1]);
 for(let i=1;i<points.length;i++){
  ctx.lineTo(points[i][0],points[i][1]);
 }
 ctx.closePath();
 ctx.fillStyle=fill;
 ctx.fill();
 if(stroke){
  ctx.strokeStyle=stroke;
  ctx.lineWidth=1;
  ctx.stroke();
 }
}

function carGradient(x,y,w,h,color){
 const grad = ctx.createLinearGradient(x,y,x+w,y+h);
 grad.addColorStop(0,"#ffffff");
 grad.addColorStop(0.18,color);
 grad.addColorStop(0.55,color);
 grad.addColorStop(1,"#171b22");
 return grad;
}

/* ======================================================
   VOITURE DÉTAILLÉE — VUE DE DESSUS
====================================================== */

function drawTopCar(cx,cy,w,h,car,isPlayer){
 const x=cx-w/2;
 const y=cy-h/2;
 const color=car.color;
 const shape=car.shape;

 ctx.save();

 /* Ombre portée */
 ctx.fillStyle="rgba(0,0,0,.5)";
 ctx.beginPath();
 ctx.ellipse(cx+3,cy+5,w*.58,h*.53,0,0,Math.PI*2);
 ctx.fill();

 /* Roues et pneus */
 const wheelW=w*.19;
 const wheelH=h*.21;
 drawWheel(x-wheelW*.15,y+h*.18,wheelW,wheelH);
 drawWheel(x+w-wheelW*.85,y+h*.18,wheelW,wheelH);
 drawWheel(x-wheelW*.15,y+h*.62,wheelW,wheelH);
 drawWheel(x+w-wheelW*.85,y+h*.62,wheelW,wheelH);

 /* Silhouettes propres à chaque modèle */
 if(shape==="lamborghini"){
  polygon([
   [cx-w*.30,y+h*.97],[cx-w*.43,y+h*.76],
   [cx-w*.48,y+h*.35],[cx-w*.28,y+h*.08],
   [cx,y],[cx+w*.28,y+h*.08],
   [cx+w*.48,y+h*.35],[cx+w*.43,y+h*.76],
   [cx+w*.30,y+h*.97]
  ],carGradient(x,y,w,h,color),"#aeb9c4");
 }else if(shape==="porsche"){
  ctx.beginPath();
  ctx.moveTo(cx-w*.30,y+h);
  ctx.bezierCurveTo(x,y+h*.78,x+w*.06,y+h*.18,cx,y+h*.04);
  ctx.bezierCurveTo(x+w*.94,y+h*.18,x+w,y+h*.78,cx+w*.30,y+h);
  ctx.closePath();
  ctx.fillStyle=carGradient(x,y,w,h,color);
  ctx.fill();
  ctx.strokeStyle="#aeb9c4";ctx.stroke();
 }else if(shape==="bugatti"){
  ctx.beginPath();
  ctx.moveTo(cx-w*.32,y+h*.98);
  ctx.bezierCurveTo(x-w*.08,y+h*.68,x+w*.05,y+h*.20,cx,y+h*.03);
  ctx.bezierCurveTo(x+w*.95,y+h*.20,x+w*1.08,y+h*.68,cx+w*.32,y+h*.98);
  ctx.closePath();
  ctx.fillStyle=carGradient(x,y,w,h,color);
  ctx.fill();
  ctx.strokeStyle="#d2d9e0";ctx.stroke();
 }else{
  polygon([
   [cx-w*.31,y+h],[cx-w*.43,y+h*.74],
   [cx-w*.46,y+h*.30],[cx-w*.25,y+h*.07],
   [cx,y+h*.02],[cx+w*.25,y+h*.07],
   [cx+w*.46,y+h*.30],[cx+w*.43,y+h*.74],
   [cx+w*.31,y+h]
  ],carGradient(x,y,w,h,color),"#b9c6d2");
 }

 /* Pare-brise avant */
 ctx.beginPath();
 if(shape==="porsche"){
  ctx.moveTo(cx-w*.25,y+h*.26);
  ctx.quadraticCurveTo(cx,y+h*.14,cx+w*.25,y+h*.26);
  ctx.lineTo(cx+w*.29,y+h*.43);
  ctx.quadraticCurveTo(cx,y+h*.49,cx-w*.29,y+h*.43);
 }else{
  ctx.moveTo(cx-w*.27,y+h*.20);
  ctx.lineTo(cx+w*.27,y+h*.20);
  ctx.lineTo(cx+w*.32,y+h*.40);
  ctx.lineTo(cx-w*.32,y+h*.40);
 }
 ctx.closePath();
 const glass=ctx.createLinearGradient(cx-w*.2,y+h*.2,cx+w*.2,y+h*.4);
 glass.addColorStop(0,"#bfefff");
 glass.addColorStop(.45,"#234b69");
 glass.addColorStop(1,"#071827");
 ctx.fillStyle=glass;ctx.fill();
 ctx.strokeStyle="#b8d9eb";ctx.lineWidth=1.1;ctx.stroke();

 /* Toit */
 ctx.beginPath();
 ctx.moveTo(cx-w*.28,y+h*.43);
 ctx.lineTo(cx+w*.28,y+h*.43);
 ctx.lineTo(cx+w*.24,y+h*.66);
 ctx.lineTo(cx-w*.24,y+h*.66);
 ctx.closePath();
 ctx.fillStyle="#101c2b";ctx.fill();
 ctx.strokeStyle="#8394a8";ctx.stroke();

 /* Vitre arrière */
 polygon([
  [cx-w*.23,y+h*.68],[cx+w*.23,y+h*.68],
  [cx+w*.29,y+h*.80],[cx-w*.29,y+h*.80]
 ],"#17384f","#a5c9db");

 /* Détails de carrosserie */
 ctx.strokeStyle="rgba(255,255,255,.6)";
 ctx.lineWidth=1;
 ctx.beginPath();
 ctx.moveTo(cx,y+h*.44);ctx.lineTo(cx,y+h*.65);
 ctx.stroke();

 /* Phares avant */
 ctx.fillStyle="#fff9cf";
 roundedRect(cx-w*.37,y+h*.08,w*.20,h*.045,2,"#fff9cf");
 roundedRect(cx+w*.17,y+h*.08,w*.20,h*.045,2,"#fff9cf");
 ctx.fillStyle="rgba(255,255,200,.2)";
 ctx.beginPath();
 ctx.moveTo(cx-w*.37,y+h*.09);
 ctx.lineTo(cx-w*.18,y+h*.09);
 ctx.lineTo(cx-w*.35,y-h*.05);
 ctx.closePath();ctx.fill();
 ctx.beginPath();
 ctx.moveTo(cx+w*.37,y+h*.09);
 ctx.lineTo(cx+w*.18,y+h*.09);
 ctx.lineTo(cx+w*.35,y-h*.05);
 ctx.closePath();ctx.fill();

 /* Feux arrière */
 ctx.fillStyle="#ff283d";
 roundedRect(cx-w*.36,y+h*.89,w*.24,h*.045,2,"#ff283d");
 roundedRect(cx+w*.12,y+h*.89,w*.24,h*.045,2,"#ff283d");

 /* Grille avant selon le modèle */
 if(shape==="bugatti"){
  ctx.strokeStyle="#d5d9e0";
  ctx.lineWidth=2;
  ctx.beginPath();
  ctx.ellipse(cx,y+h*.09,w*.10,h*.055,0,0,Math.PI*2);
  ctx.stroke();
 }else{
  ctx.fillStyle="#14171b";
  polygon([
   [cx-w*.12,y+h*.02],[cx+w*.12,y+h*.02],
   [cx+w*.08,y+h*.10],[cx-w*.08,y+h*.10]
  ],"#14171b");
 }

 /* Aileron sport */
 if(shape==="lamborghini" || shape==="ferrari"){
  ctx.fillStyle="#1b2028";
  roundedRect(cx-w*.34,y+h*.79,w*.68,h*.035,2,"#1b2028");
 }

 /* Reflet de carrosserie */
 ctx.strokeStyle="rgba(255,255,255,.42)";
 ctx.lineWidth=1.3;
 ctx.beginPath();
 ctx.moveTo(cx-w*.31,y+h*.24);
 ctx.lineTo(cx-w*.34,y+h*.67);
 ctx.stroke();

 if(isPlayer){
  ctx.fillStyle="#fff";
  ctx.font="bold 9px Arial";
  ctx.textAlign="center";
  ctx.fillText("VIP",cx,y+h*.56);
 }

 ctx.restore();
}

/* ======================================================
   VOITURE — PERSPECTIVE ARRIÈRE
====================================================== */

function drawRearCar(cx,cy,w,h,car,isPlayer){
 const x=cx-w/2;
 const y=cy-h/2;
 const color=car.color;
 const shape=car.shape;

 ctx.save();

 /* Ombre */
 ctx.fillStyle="rgba(0,0,0,.52)";
 ctx.beginPath();
 ctx.ellipse(cx,cy+h*.38,w*.72,h*.24,0,0,Math.PI*2);
 ctx.fill();

 /* Pneus */
 drawWheel(x-w*.08,y+h*.40,w*.23,h*.40);
 drawWheel(x+w*.85,y+h*.40,w*.23,h*.40);

 /* Carrosserie arrière : silhouette différente par modèle */
 if(shape==="porsche"){
  ctx.beginPath();
  ctx.moveTo(cx-w*.46,y+h*.17);
  ctx.quadraticCurveTo(x-w*.02,y-h*.02,x+w*.13,y+h*.05);
  ctx.lineTo(x+w*.87,y+h*.05);
  ctx.quadraticCurveTo(x+w*1.02,y-h*.02,cx+w*.46,y+h*.17);
  ctx.lineTo(cx+w*.39,y+h*.83);
  ctx.quadraticCurveTo(cx,y+h*1.03,cx-w*.39,y+h*.83);
  ctx.closePath();
  ctx.fillStyle=carGradient(x,y,w,h,color);ctx.fill();
  ctx.strokeStyle="#c6d0d9";ctx.stroke();
 }else if(shape==="bugatti"){
  polygon([
   [cx-w*.49,y+h*.22],[cx-w*.43,y+h*.06],
   [cx-w*.20,y],[cx+w*.20,y],
   [cx+w*.43,y+h*.06],[cx+w*.49,y+h*.22],
   [cx+w*.43,y+h*.83],[cx+w*.28,y+h*.97],
   [cx-w*.28,y+h*.97],[cx-w*.43,y+h*.83]
  ],carGradient(x,y,w,h,color),"#d5dce5");
 }else if(shape==="lamborghini"){
  polygon([
   [cx-w*.43,y+h*.08],[cx+w*.43,y+h*.08],
   [cx+w*.49,y+h*.33],[cx+w*.40,y+h*.93],
   [cx-w*.40,y+h*.93],[cx-w*.49,y+h*.33]
  ],carGradient(x,y,w,h,color),"#d5dce5");
 }else{
  polygon([
   [cx-w*.39,y+h*.06],[cx+w*.39,y+h*.06],
   [cx+w*.47,y+h*.25],[cx+w*.42,y+h*.91],
   [cx-w*.42,y+h*.91],[cx-w*.47,y+h*.25]
  ],carGradient(x,y,w,h,color),"#d5dce5");
 }

 /* Lunette arrière */
 polygon([
  [cx-w*.28,y+h*.14],[cx+w*.28,y+h*.14],
  [cx+w*.34,y+h*.40],[cx-w*.34,y+h*.40]
 ],"#18384e","#c0e4f4");

 /* Feux arrière */
 ctx.fillStyle="#ff1f38";
 roundedRect(cx-w*.42,y+h*.52,w*.23,h*.09,3,"#ff1f38");
 roundedRect(cx+w*.19,y+h*.52,w*.23,h*.09,3,"#ff1f38");

 /* Bande lumineuse arrière */
 ctx.fillStyle="#ff5a62";
 roundedRect(cx-w*.20,y+h*.55,w*.40,h*.025,2,"#ff5a62");

 /* Plaque */
 roundedRect(cx-w*.15,y+h*.67,w*.30,h*.12,3,"#e7edf1","#8998a4");
 ctx.fillStyle="#152331";
 ctx.font="bold 8px Arial";
 ctx.textAlign="center";
 ctx.fillText("AHC VIP",cx,y+h*.75);

 /* Diffuseur et échappements */
 ctx.fillStyle="#121820";
 roundedRect(cx-w*.29,y+h*.82,w*.58,h*.10,3,"#121820");
 ctx.fillStyle="#929da7";
 roundedRect(cx-w*.22,y+h*.87,w*.11,h*.05,2,"#929da7");
 roundedRect(cx+w*.11,y+h*.87,w*.11,h*.05,2,"#929da7");

 /* Aileron selon le modèle */
 if(shape==="ferrari" || shape==="lamborghini"){
  ctx.fillStyle="#202731";
  roundedRect(cx-w*.43,y+h*.07,w*.86,h*.045,3,"#202731");
 }
 if(isPlayer){
  ctx.fillStyle="#fff";
  ctx.font="bold 10px Arial";
  ctx.textAlign="center";
  ctx.fillText("VIP",cx,y+h*.33);
 }

 ctx.restore();
}

/* ======================================================
   DESSIN DE LA ROUTE ET DU CIRCUIT
====================================================== */

function drawRoadTop(){
 ctx.fillStyle="#287342";
 ctx.fillRect(0,0,W,H);

 ctx.fillStyle="#343b43";
 ctx.fillRect(28,0,W-56,H);

 ctx.fillStyle="#f5f5f5";
 ctx.fillRect(30,0,3,H);
 ctx.fillRect(W-33,0,3,H);

 const laneW=(W-56)/3;
 ctx.strokeStyle="#e8e9ec";
 ctx.lineWidth=2;
 ctx.setLineDash([28,25]);
 ctx.lineDashOffset=-roadOffset;
 for(let i=1;i<3;i++){
  ctx.beginPath();
  ctx.moveTo(28+laneW*i,0);
  ctx.lineTo(28+laneW*i,H);
  ctx.stroke();
 }
 ctx.setLineDash([]);

 for(let y=-50;y<H+50;y+=65){
  const yy=(y+roadOffset*1.5)%(H+65)-25;
  ctx.fillStyle="#f9f9f9";
  ctx.fillRect(28,yy,5,24);
  ctx.fillRect(W-33,yy,5,24);
 }
}

function drawRoadRear(){
 ctx.fillStyle="#173b2a";
 ctx.fillRect(0,0,W,H);

 /* Horizon */
 const horizon=H*.32;
 const roadBottom=W*.98;
 const roadTop=W*.22;

 polygon([
  [W/2-roadTop/2,horizon],
  [W/2+roadTop/2,horizon],
  [W/2+roadBottom/2,H],
  [W/2-roadBottom/2,H]
 ],"#343b43");

 /* Bandes latérales */
 ctx.strokeStyle="#e5e9ed";
 ctx.lineWidth=3;
 ctx.beginPath();
 ctx.moveTo(W/2-roadTop/2,horizon);
 ctx.lineTo(W/2-roadBottom/2,H);
 ctx.moveTo(W/2+roadTop/2,horizon);
 ctx.lineTo(W/2+roadBottom/2,H);
 ctx.stroke();

 /* Lignes de voies en perspective */
 for(let lane=1;lane<3;lane++){
  const t=lane/3;
  ctx.strokeStyle="#dce2e6";
  ctx.lineWidth=2;
  ctx.beginPath();
  ctx.moveTo(W/2-roadTop/2+roadTop*t,horizon);
  ctx.lineTo(W/2-roadBottom/2+roadBottom*t,H);
  ctx.stroke();
 }

 /* Marques de route en mouvement */
 for(let i=0;i<9;i++){
  const t=((i/9)+(roadOffset/380))%1;
  const y=horizon+(H-horizon)*t*t;
  const roadWidth=roadTop+(roadBottom-roadTop)*t;
  ctx.fillStyle="#f5f5f5";
  ctx.fillRect(W/2-roadWidth/2,y,roadWidth*.035,Math.max(2,2+t*10));
  ctx.fillRect(W/2+roadWidth/2-roadWidth*.035,y,roadWidth*.035,Math.max(2,2+t*10));
 }
}

function drawObstacle(o){
 if(mode==="top"){
  const cx=laneCenter(o.lane);
  drawTopCar(cx,o.y,36,59,{
   color:o.color,
   shape:"porsche"
  },false);
 }else{
  const t=Math.max(0,Math.min(1,(o.y-H*.32)/(H*.68)));
  const w=18+t*34;
  const h=25+t*43;
  const cx=laneCenter(o.lane);
  drawRearCar(cx,o.y,w,h,{
   color:o.color,
   shape:"porsche"
  },false);
 }
}

function drawScene(){
 if(!ctx) return;

 ctx.clearRect(0,0,W,H);

 if(mode==="top"){
  drawRoadTop();
 }else{
  drawRoadRear();
 }

 obstacles.forEach(drawObstacle);

 const car=getSelectedCar();

 if(mode==="top"){
  const px=playerX || laneCenter(playerLane);
  drawTopCar(px,H-100,49,83,car,true);
 }else{
  const px=playerX || laneCenter(playerLane);
  drawRearCar(px,H-104,65,100,car,true);
 }

 if(crashFlash>0){
  ctx.fillStyle="rgba(255,20,40,.28)";
  ctx.fillRect(0,0,W,H);
 }

 ctx.fillStyle="#fff";
 ctx.font="bold 12px Arial";
 ctx.textAlign="left";
 ctx.fillText(car.brand+" "+car.value,10,20);
}

/* ======================================================
   COMMANDES ET DÉMARRAGE
====================================================== */

function startRace(){
 if(running) return;

 running=true;
 raceScore=0;
 raceLevel=1;
 playerLane=1;
 playerX=laneCenter(playerLane);
 obstacles=[];
 roadOffset=0;
 currentSpeed=0;
 turboTime=0;
 crashFlash=0;
 lastObstacleTime=0;
 lastFrame=performance.now();

 document.getElementById("raceMessage").textContent =
  "🏁 Course lancée ! Évite les voitures et collecte des points.";

 document.getElementById("startButton").textContent="🏁 EN COURSE";

 cancelAnimationFrame(animation);
 animation=requestAnimationFrame(gameLoop);
}

function closeRace(){
 running=false;
 accelerating=false;
 braking=false;
 turboTime=0;
 cancelAnimationFrame(animation);

 document.getElementById("startButton").textContent="▶️ DÉPART";
 document.getElementById("raceMessage").textContent =
  "Course arrêtée. Score obtenu : "+raceScore+".";
 drawScene();
}

function moveCar(direction){
 if(!running) return;

 const car=getSelectedCar();
 playerLane=Math.max(0,Math.min(2,playerLane+direction));

 const target=laneCenter(playerLane);
 const handlingFactor=0.45+(car.handling/10)*0.55;

 playerX += (target-playerX)*handlingFactor;

 if(playerLane===0 || playerLane===2){
  playerX=Math.max(42,Math.min(W-42,playerX));
 }
}

function accelerate(active){
 accelerating=Boolean(active);
}

function brake(active){
 braking=Boolean(active);
}

function turbo(){
 if(!running) return;

 if(turboTime<=0){
  turboTime=95;
  document.getElementById("raceMessage").textContent =
   "🚀 TURBO activé !";
 }
}

function bindHoldButton(id,onStart,onEnd){
 const button=document.getElementById(id);

 button.addEventListener("pointerdown",function(e){
  e.preventDefault();
  onStart();
  if(button.setPointerCapture){
   try{button.setPointerCapture(e.pointerId);}catch(err){}
  }
 });

 ["pointerup","pointercancel","lostpointercapture","pointerleave"].forEach(eventName=>{
  button.addEventListener(eventName,function(){
   onEnd();
  });
 });
}

bindHoldButton("accelerateButton",()=>accelerate(true),()=>accelerate(false));
bindHoldButton("brakeButton",()=>brake(true),()=>brake(false));

document.getElementById("turboButton").addEventListener("click",turbo);

/* ======================================================
   COLLISIONS ET BOUCLE DU JEU
====================================================== */

function checkCollision(o){
 if(o.lane!==playerLane) return false;

 const playerY=H-100;
 const distance=Math.abs(o.y-playerY);

 return distance < (mode==="top" ? 58 : 63);
}

function endCrash(){
 running=false;
 accelerating=false;
 braking=false;
 crashFlash=1;

 document.getElementById("startButton").textContent="▶️ REJOUER";
 document.getElementById("raceMessage").textContent =
  "💥 Collision ! Score de la course : "+raceScore+
  ". Appuie sur REJOUER pour recommencer.";

 if(raceScore>bestScore){
  bestScore=raceScore;
 }
 earnPoints(Math.max(0,Math.floor(raceScore/5)));

 drawScene();
}

function gameLoop(timestamp){
 if(!running) return;

 const delta=Math.min((timestamp-lastFrame)/16.67,2);
 lastFrame=timestamp;

 const car=getSelectedCar();

 let speed=2.3+car.speed*.28+raceLevel*.12;

 if(accelerating){
  speed += car.accel*.16;
 }
 if(turboTime>0){
  speed += 3.8;
  turboTime -= delta;
 }
 if(braking){
  speed *= 0.45;
 }

 speed=Math.min(speed,12.5);
 currentSpeed=Math.max(0,Math.round(speed*10));

 roadOffset += speed*delta;
 raceScore += Math.max(0,Math.floor(speed*delta/3));

 raceLevel=Math.floor(raceScore/250)+1;

 document.getElementById("raceScore").textContent=raceScore;
 document.getElementById("raceLevel").textContent=raceLevel;
 document.getElementById("raceSpeed").textContent=currentSpeed;

 /* Déplacement progressif selon la maniabilité du modèle */
 const targetX=laneCenter(playerLane);
 const response=0.15+(car.handling/10)*0.22;
 playerX+=(targetX-playerX)*response;

 /* Apparition progressive des obstacles */
 if(timestamp-lastObstacleTime>Math.max(500,1350-raceLevel*35)){
  lastObstacleTime=timestamp;
  const lane=Math.floor(Math.random()*laneCount);

  obstacles.push({
   lane:lane,
   y:mode==="top" ? -60 : H*.33,
   color:["#ff3030","#f5f5f5","#ffc857","#16c784"][Math.floor(Math.random()*4)]
  });
 }

 obstacles.forEach(o=>{
  o.y += speed*delta*(mode==="top" ? 1 : 1.15);
 });

 obstacles=obstacles.filter(o=>o.y<H+100);

 for(const obstacle of obstacles){
  if(checkCollision(obstacle)){
   endCrash();
   return;
  }
 }

 crashFlash=Math.max(0,crashFlash-.03);
 drawScene();

 animation=requestAnimationFrame(gameLoop);
}

/* ======================================================
   COMMANDES CLAVIER
====================================================== */

document.addEventListener("keydown",function(e){
 if(e.repeat && ["ArrowLeft","ArrowRight","ArrowUp","ArrowDown"," "].includes(e.key)){
  e.preventDefault();
 }

 if(e.key==="ArrowLeft") moveCar(-1);
 if(e.key==="ArrowRight") moveCar(1);
 if(e.key==="ArrowUp") accelerate(true);
 if(e.key==="ArrowDown") brake(true);
 if(e.key===" "){
  e.preventDefault();
  turbo();
 }
});

document.addEventListener("keyup",function(e){
 if(e.key==="ArrowUp") accelerate(false);
 if(e.key==="ArrowDown") brake(false);
});

/* ======================================================
   BOUTIQUE VIRTUELLE
====================================================== */

function buyItem(item,cost){
 const msg=document.getElementById("shopMessage");

 if(ownedItems.includes(item)){
  msg.textContent="Cet article est déjà débloqué.";
  return;
 }

 if(points<cost){
  msg.textContent="Points insuffisants. Tu possèdes "+points+" points.";
  return;
 }

 points-=cost;
 ownedItems.push(item);

 if(item==="boost"){
  purchasedBoost=true;
 }

 saveGame();

 msg.textContent="✅ Article virtuel débloqué avec succès !";
}

/* ======================================================
   INITIALISATION
====================================================== */

document.getElementById("carBrand").addEventListener("change",function(){
 updateModelOptions();
 updateGarage();
});

document.getElementById("carModel").addEventListener("change",updateGarage);
document.getElementById("carColor").addEventListener("change",updateGarage);

updateDashboard();
updateModelOptions();
renderQuiz();
updateGarage();
drawScene();
</script>

</body>
</html>