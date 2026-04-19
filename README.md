<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AI Story Universe 200 MULTI-GENRE</title>

<style>
body{
margin:0;
font-family:Arial;
background:#0b0f1a;
color:white;
}

header{
padding:15px;
text-align:center;
background:linear-gradient(135deg,#00e5ff,#7a00ff);
font-weight:bold;
}

.container{
padding:10px;
text-align:center;
}

input{
width:90%;
padding:10px;
border-radius:10px;
border:none;
background:#1c1f2b;
color:white;
margin:5px;
}

.grid{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(160px,1fr));
gap:10px;
padding:10px;
}

.card{
background:#1c1f2b;
padding:12px;
border-radius:12px;
cursor:pointer;
transition:0.2s;
}
.card:hover{
transform:scale(1.05);
background:#2a2f45;
}

.view{
position:fixed;
inset:0;
background:#0b0f1a;
display:none;
flex-direction:column;
padding:10px;
overflow:auto;
}

.content{
background:#111;
padding:12px;
border-radius:12px;
white-space:pre-line;
line-height:1.6;
}

button{
padding:8px;
margin:5px;
border:none;
border-radius:8px;
cursor:pointer;
}

.play{background:#00ff88}
.stop{background:#ff9900}
.back{background:#ff004c;color:#fff}
.ai{background:#00e5ff}
.fav{background:#ffcc00}
</style>
</head>

<body>

<header>📖 AI STORY UNIVERSE 200 MULTI-GENRE ENGINE</header>

<div class="container">

<input id="prompt" placeholder="اكتب فكرة القصة...">
<button class="ai" onclick="generateAI()">🧠 AI Generate</button>

</div>

<div class="grid" id="grid"></div>

<div class="view" id="view">

<button class="back" onclick="closeView()">⬅ Back</button>
<button onclick="prev()">⬅ Prev</button>
<button onclick="next()">Next ➡</button>
<button class="play" onclick="speak()">▶ Play</button>
<button class="stop" onclick="stopSpeak()">⏹ Stop</button>
<button class="fav" onclick="toggleFav()">❤️ Favorite</button>

<h2 id="title"></h2>
<div class="content" id="text"></div>
<div id="progress"></div>

</div>

<script>

let books=[];
let current=null;
let chapter=0;
let synth=window.speechSynthesis;
let favorites=[];

// =========================
// 🎧 BETTER VOICE ENGINE
// =========================
let voices=[];

speechSynthesis.onvoiceschanged=()=>{
voices=speechSynthesis.getVoices();
};

function getBestVoice(lang="en"){
let list = voices.filter(v=>v.lang.startsWith(lang));
return list.find(v=>
v.name.toLowerCase().includes("google") ||
v.name.toLowerCase().includes("microsoft") ||
v.name.toLowerCase().includes("natural")
) || list[0] || voices[0];
}

// =========================
// 🧠 MULTI-GENRE STORY ENGINE
// =========================
function createStory(topic){

let genres=["fantasy","sci-fi","mystery","war","survival","horror"];
let genre=genres[Math.floor(Math.random()*genres.length)];

let en=[];
let state={power:1,fear:1,mystery:0};

for(let i=1;i<=60;i++){

let event="";

if(genre==="fantasy"){
let e=["finds magic rune","meets dragon","opens portal","awakens ancient king"];
event=e[Math.floor(Math.random()*e.length)];
}
else if(genre==="sci-fi"){
let e=["hacks AI system","travels wormhole","detects alien signal","breaks gravity rules"];
event=e[Math.floor(Math.random()*e.length)];
}
else if(genre==="mystery"){
let e=["finds hidden clue","opens secret door","receives coded message","solves puzzle"];
event=e[Math.floor(Math.random()*e.length)];
}
else if(genre==="war"){
let e=["joins battle","leads army","loses territory","survives explosion"];
event=e[Math.floor(Math.random()*e.length)];
}
else if(genre==="survival"){
let e=["runs out of food","builds shelter","faces storm","explores danger"];
event=e[Math.floor(Math.random()*e.length)];
}
else{
let e=["hears voices","sees shadow","finds cursed place","feels presence"];
event=e[Math.floor(Math.random()*e.length)];
}

state.mystery++;

en.push(`
Chapter ${i}

(${genre.toUpperCase()})
In ${topic}, the hero ${event}.
The world reacts differently every time.
Power:${state.power} Fear:${state.fear} Mystery:${state.mystery}
`);
}

return en;
}

// =========================
// 📚 200 STORIES
// =========================
for(let i=1;i<=200;i++){
books.push({
id:i,
title:"Story "+i,
data:createStory("World "+i)
});
}

// =========================
// 🧠 AI GENERATOR
// =========================
function generateAI(){

let t=document.getElementById("prompt").value;
if(!t) return alert("اكتب فكرة");

books.unshift({
id:Date.now(),
title:"🧠 AI: "+t,
data:createStory(t)
});

render();
}

// =========================
// UI
// =========================
function render(list=books){
let g=document.getElementById("grid");
g.innerHTML="";
list.forEach(b=>{
g.innerHTML+=`<div class="card" onclick="openBook(${b.id})">${b.title}</div>`;
});
}
render();

// =========================
// OPEN
// =========================
function openBook(id){
current=books.find(b=>b.id==id);
chapter=0;
document.getElementById("view").style.display="flex";
update();
}

// =========================
// UPDATE
// =========================
function update(){
document.getElementById("title").innerText=
current.title+" - Chapter "+(chapter+1);

document.getElementById("text").innerText=
current.data[chapter];

document.getElementById("progress").innerText=
`Progress: ${chapter+1} / ${current.data.length}`;
}

// =========================
// NEXT / PREV
// =========================
function next(){
if(chapter<current.data.length-1){
chapter++;
update();
}
}

function prev(){
if(chapter>0){
chapter--;
update();
}
}

// =========================
// CLOSE
// =========================
function closeView(){
document.getElementById("view").style.display="none";
stopSpeak();
}

// =========================
// FAVORITES
// =========================
function toggleFav(){
if(!favorites.includes(current.id)){
favorites.push(current.id);
alert("❤️ Added");
}else{
alert("Already exists");
}
}

// =========================
// 🔊 IMPROVED SPEECH
// =========================
function speak(){
stopSpeak();

let text=current.data[chapter];

let msg=new SpeechSynthesisUtterance(text);

msg.voice=getBestVoice("en");
msg.lang="en-US";
msg.rate=0.88;
msg.pitch=1.05;
msg.volume=1;

synth.speak(msg);
}

function stopSpeak(){
synth.cancel();
}

</script>

</body>
</html>
