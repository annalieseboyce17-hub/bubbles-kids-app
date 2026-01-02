# bubbles-kids-app
kids learning game
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Bubbles & Stickers Kids App</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
body { margin:0; font-family:Comic Sans MS, Arial; overflow:hidden; background: linear-gradient(to top, #a8edea, #fed6e3);}
.screen { display:none; width:100%; height:100%; position:absolute; top:0; left:0;}
.active { display:block;}
button { font-size:24px; padding:20px; margin:10px; border-radius:15px; border:none; cursor:pointer;}
#home { display:flex; flex-direction:column; align-items:center; justify-content:center; }
#coins { position:fixed; top:10px; right:10px; background:gold; padding:10px 16px; border-radius:20px; font-size:20px; box-shadow:0 0 8px rgba(0,0,0,.3);}
.bubble, .balloon { position:absolute; border-radius:50%; color:white; display:flex; align-items:center; justify-content:center; font-weight:bold; cursor:pointer; user-select:none; text-align:center; }
#stickers { position:fixed; bottom:0; width:100%; background:rgba(255,255,255,.9); display:flex; gap:10px; padding:10px; overflow-x:auto;}
.sticker { width:60px; height:60px; border-radius:10px; background-size:cover; background-position:center; border:3px solid #ccc;}
#matchingArea { display:flex; flex-wrap:wrap; justify-content:center; align-items:center; gap:20px; padding:20px;}
.matchItem { width:100px; height:100px; border:2px dashed #555; display:flex; align-items:center; justify-content:center; font-size:24px; cursor:pointer; }
.matchCard { width:100px; height:100px; display:flex; align-items:center; justify-content:center; font-size:24px; background:#fff; border-radius:10px; cursor:pointer; user-select:none;}
</style>
</head>
<body>

<div id="coins">🪙 <span id="coinCount">0</span></div>

<!-- HOME SCREEN -->
<div id="home" class="screen active">
<h1>🎉 Bubbles & Stickers 🎉</h1>
<button onclick="showScreen('colors')">Colors</button>
<button onclick="showScreen('shapes')">Shapes</button>
<button onclick="showScreen('bubbles')">Pop Bubbles</button>
<button onclick="showScreen('balloons')">Pop Balloons</button>
<button onclick="showScreen('matching')">Matching Game</button>
<button onclick="showScreen('stickersScreen')">Sticker Collection</button>
</div>

<!-- COLORS GAME -->
<div id="colors" class="screen">
<h2>Colors Game</h2>
<div id="colorArea" style="display:flex; flex-wrap:wrap; justify-content:center; gap:20px;"></div>
<button onclick="goHome()">Back</button>
</div>

<!-- SHAPES GAME -->
<div id="shapes" class="screen">
<h2>Shapes Game</h2>
<div id="shapeArea" style="display:flex; flex-wrap:wrap; justify-content:center; gap:20px;"></div>
<button onclick="goHome()">Back</button>
</div>

<!-- BUBBLE POP -->
<div id="bubbles" class="screen">
<h2>Pop Bubbles</h2>
<button onclick="goHome()">Back</button>
</div>

<!-- BALLOON POP -->
<div id="balloons" class="screen">
<h2>Pop Balloons</h2>
<button onclick="goHome()">Back</button>
</div>

<!-- MATCHING GAME -->
<div id="matching" class="screen">
<h2>Matching Game</h2>
<div id="matchingArea"></div>
<button onclick="goHome()">Back</button>
</div>

<!-- STICKER COLLECTION -->
<div id="stickersScreen" class="screen">
<h2>Sticker Collection</h2>
<div id="stickers"></div>
<button onclick="goHome()">Back</button>
</div>

<script>
let coins = 0;
let unlockedStickers = [];
let lockedStickers = [
{name:'🕷️', cost:5}, {name:'🐞', cost:5}, {name:'🦋', cost:10}, {name:'🐝', cost:10},
{name:'🦖', cost:5}, {name:'🧸', cost:5}, {name:'🐰', cost:5}
];

const colors = ['Red','Blue','Yellow','Green','Purple','Orange'];
const shapes = ['Circle','Square','Triangle','Star','Heart','Rectangle'];

function showScreen(id){
document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
document.getElementById(id).classList.add('active');
if(id==='colors') loadColors();
if(id==='shapes') loadShapes();
if(id==='bubbles') startBubbles();
if(id==='balloons') startBalloons();
if(id==='matching') loadMatching();
if(id==='stickersScreen') showStickers();
}

function goHome(){
showScreen('home');
stopBubbles();
stopBalloons();
}

function addCoins(n){
coins += n;
document.getElementById('coinCount').textContent = coins;
}

//// COLORS GAME
function loadColors(){
const area = document.getElementById('colorArea');
area.innerHTML='';
colors.forEach(c=>{
const btn = document.createElement('button');
btn.textContent=c;
btn.style.background=c.toLowerCase();
btn.style.color='white';
btn.style.fontWeight='bold';
btn.onclick=()=>{addCoins(1); speak(c);}
area.appendChild(btn);
});
}

//// SHAPES GAME
function loadShapes(){
const area = document.getElementById('shapeArea');
area.innerHTML='';
shapes.forEach(s=>{
const btn = document.createElement('button');
btn.textContent=s;
btn.style.background='lightblue';
btn.style.color='black';
btn.onclick=()=>{addCoins(1); speak(s);}
area.appendChild(btn);
});
}

//// SPEAK FUNCTION
function speak(text){
if('speechSynthesis' in window){
const msg = new SpeechSynthesisUtterance(text);
window.speechSynthesis.speak(msg);
}
}

//// BUBBLE POP
let bubbleInterval;
function startBubbles(){
const container=document.getElementById('bubbles');
bubbleInterval=setInterval(()=>{
const b=document.createElement('div');
b.className='bubble';
b.textContent=colors[Math.floor(Math.random()*colors.length)];
b.style.width=b.style.height='60px';
b.style.background='hsl('+Math.random()*360+',70%,60%)';
b.style.left=Math.random()*(window.innerWidth-60)+'px';
b.style.top=window.innerHeight+'px';
container.appendChild(b);
let y=window.innerHeight;
const float=setInterval(()=>{
y-=2;
b.style.top=y+'px';
if(y<-100){clearInterval(float); b.remove();}
},16);
b.onclick=()=>{
addCoins(1);
speak(b.textContent);
b.remove();
}
},1000);
}

function stopBubbles(){ clearInterval(bubbleInterval); document.querySelectorAll('.bubble').forEach(b=>b.remove()); }

//// BALLOON POP
let balloonInterval;
function startBalloons(){
const container=document.getElementById('balloons');
balloonInterval=setInterval(()=>{
const b=document.createElement('div');
b.className='balloon';
b.textContent=shapes[Math.floor(Math.random()*shapes.length)];
b.style.width=b.style.height='60px';
b.style.background='hsl('+Math.random()*360+',80%,60%)';
b.style.left=Math.random()*(window.innerWidth-60)+'px';
b.style.top=window.innerHeight+'px';
container.appendChild(b);
let y=window.innerHeight;
const float=setInterval(()=>{
y-=1.5;
b.style.top=y+'px';
if(y<-100){clearInterval(float); b.remove();}
},16);
b.onclick=()=>{
addCoins(1);
speak(b.textContent);
b.remove();
}
},1200);
}

function stopBalloons(){ clearInterval(balloonInterval); document.querySelectorAll('.balloon').forEach(b=>b.remove()); }

//// MATCHING GAME
const animalPairs=[{name:'🐘',shape:'Elephant'},{name:'🦁',shape:'Lion'},{name:'🐶',shape:'Dog'}];
let selectedCard=null;
function loadMatching(){
const area=document.getElementById('matchingArea');
area.innerHTML='';
let cards=[];
animalPairs.forEach(p=>{
cards.push({type:'card',val:p.name});
cards.push({type:'slot',val:p.name});
});
cards.sort(()=>Math.random()-0.5);
cards.forEach(c=>{
if(c.type==='card'){
const div=document.createElement('div');
div.className='matchCard';
div.textContent=c.val;
div.draggable=true;
div.ondragstart=(e)=>{selectedCard=c.val;}
area.appendChild(div);
} else {
const div=document.createElement('div');
div.className='matchItem';
div.textContent=c.val;
div.ondragover=(e)=>e.preventDefault();
div.ondrop=(e)=>{
if(selectedCard===c.val){ addCoins(2); div.style.background='lightgreen'; speak('Correct!'); }
}
area.appendChild(div);
}
});
}

//// STICKER COLLECTION
function showStickers(){
const panel=document.getElementById('stickers');
panel.innerHTML='';
lockedStickers.forEach(s=>{
const div=document.createElement('div');
div.className='sticker';
div.textContent=s.name;
div.style.fontSize='36px';
div.style.opacity = coins>=s.cost ? '1':'0.4';
div.onclick=()=>{
if(coins>=s.cost){
coins-=s.cost;
unlockedStickers.push(s);
lockedStickers.splice(lockedStickers.indexOf(s),1);
document.getElementById('coinCount').textContent=coins;
showStickers();
}
}
panel.appendChild(div);
});
unlockedStickers.forEach(s=>{
const div=document.createElement('div');
div.className='sticker';
div.textContent=s.name;
div.style.fontSize='36px';
panel.appendChild(div);
});
}
</script>

</body>
</html>
