# bubbles-kids-app
kids learning game
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Bubbles & Stickers</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
body {
margin: 0;
overflow: hidden;
font-family: Comic Sans MS, Arial;
background: linear-gradient(#a8edea, #fed6e3);
}

#coins {
position: fixed;
top: 10px;
right: 10px;
background: gold;
padding: 10px 16px;
border-radius: 20px;
font-size: 20px;
box-shadow: 0 0 8px rgba(0,0,0,.3);
}

.bubble {
position: absolute;
border-radius: 50%;
color: white;
display: flex;
align-items: center;
justify-content: center;
font-size: 18px;
cursor: pointer;
user-select: none;
}

#stickers {
position: fixed;
bottom: 0;
width: 100%;
background: rgba(255,255,255,.9);
display: flex;
gap: 10px;
padding: 10px;
overflow-x: auto;
}

.sticker {
width: 60px;
height: 60px;
border-radius: 10px;
background-size: cover;
background-position: center;
border: 3px solid #ccc;
}
</style>
</head>

<body>

<div id="coins">🪙 <span id="coinCount">0</span></div>
<div id="stickers"></div>

<script>
let coins = 0;

const words = [
{ text: "Red", sound: "pop" },
{ text: "Blue", sound: "pop" },
{ text: "Circle", sound: "pop" },
{ text: "Square", sound: "pop" },
{ text: "Apple", sound: "pop" }
];

const lockedStickers = [
{ name: "🐞", cost: 5 },
{ name: "🐝", cost: 10 },
{ name: "🧸", cost: 5 },
{ name: "🐰", cost: 10 }
];

const unlocked = [];

function makeBubble() {
const bubble = document.createElement("div");
const item = words[Math.floor(Math.random() * words.length)];

bubble.className = "bubble";
bubble.textContent = item.text;
bubble.style.width = bubble.style.height = Math.random()*40+50+"px";
bubble.style.left = Math.random()*(window.innerWidth-80)+"px";
bubble.style.top = window.innerHeight + "px";
bubble.style.background = `hsl(${Math.random()*360},70%,60%)`;

document.body.appendChild(bubble);

let y = window.innerHeight;
const float = setInterval(()=>{
y -= 1.5;
bubble.style.top = y+"px";
if(y < -100){
bubble.remove();
clearInterval(float);
}
},16);

bubble.onclick = () => {
coins++;
document.getElementById("coinCount").textContent = coins;
bubble.remove();
showStickers();
};
}

function showStickers(){
const panel = document.getElementById("stickers");
panel.innerHTML = "";

lockedStickers.forEach(s=>{
const d = document.createElement("div");
d.className = "sticker";
d.textContent = s.name;
d.style.fontSize = "36px";
d.style.opacity = coins >= s.cost ? "1" : "0.4";

d.onclick = ()=>{
if(coins >= s.cost){
coins -= s.cost;
unlocked.push(s);
lockedStickers.splice(lockedStickers.indexOf(s),1);
document.getElementById("coinCount").textContent = coins;
showStickers();
}
};
panel.appendChild(d);
});

unlocked.forEach(s=>{
const d = document.createElement("div");
d.className = "sticker";
d.textContent = s.name;
d.style.fontSize = "36px";
panel.appendChild(d);
});
}

setInterval(makeBubble, 1500);
</script>

</body>
</html>
