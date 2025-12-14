<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Advanced Calculator + Unit Converter</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&family=Orbitron:wght@500;600&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
<style>
*{
    font-family:'Poppins', sans-serif;
    letter-spacing:0.4px;
}
body{
    background:linear-gradient(135deg,#0f2027,#203a43,#2c5364);
    margin:0;
    height:100vh;
    display:flex;
    justify-content:center;
    align-items:center;
}
.container{
    background:rgba(255,255,255,0.1);
    backdrop-filter:blur(15px);
    border-radius:20px;
    padding:25px;
    display:flex;
    gap:25px;
    box-shadow:0 25px 60px rgba(0,0,0,0.6);
}
.card{
    background:rgba(0,0,0,0.55);
    border-radius:18px;
    padding:20px;
    width:270px;
}
.card h3{color:#fff;text-align:center;margin-bottom:15px;}
#display{
    width:100%;
    height:55px;
    font-size:26px;
    font-family:'Orbitron', 'Poppins', sans-serif;
    font-weight:600;
    text-align:right;
    padding:10px;
    border-radius:12px;
    border:none;
    background:#000;
    color:#00ffcc;
    margin-bottom:15px;
}
.buttons{display:grid;grid-template-columns:repeat(4,1fr);gap:10px;}
button{
    height:55px;
    border:none;
    border-radius:14px;
    font-size:18px;
    font-weight:600;
    cursor:pointer;
    background:rgba(255,255,255,0.1);
    color:#fff;
    transition:0.25s;
}
button:hover{transform:scale(1.05);background:rgba(255,255,255,0.25);} 
.operator{background:#ff9800;}
.equal{background:#4caf50;grid-column:span 2;}
.clear{background:#f44336;grid-column:span 2;}
.unit select,.unit input{
    width:100%;padding:10px;margin-bottom:12px;border-radius:12px;border:none;
}
</style>
</head>
<body>

<div class="container">

<div class="card">
<h3><i class="fa-solid fa-calculator"></i> Calculator</h3>
<input type="text" id="display" disabled>
<div class="buttons">
<button class="clear" onclick="clr()"><i class="fa-solid fa-trash"></i> C</button>
<button onclick="del()"><i class="fa-solid fa-delete-left"></i></button>
<button onclick="add('7')">7</button>
<button onclick="add('8')">8</button>
<button onclick="add('9')">9</button>
<button class="operator" onclick="add('/')"><i class="fa-solid fa-divide"></i></button>
<button onclick="add('4')">4</button>
<button onclick="add('5')">5</button>
<button onclick="add('6')">6</button>
<button class="operator" onclick="add('*')"><i class="fa-solid fa-xmark"></i></button>
<button onclick="add('1')">1</button>
<button onclick="add('2')">2</button>
<button onclick="add('3')">3</button>
<button class="operator" onclick="add('-')"><i class="fa-solid fa-minus"></i></button>
<button onclick="add('0')">0</button>
<button onclick="add('.')">.</button>
<button class="equal" onclick="calc()"><i class="fa-solid fa-equals"></i></button>
<button class="operator" onclick="add('+')"><i class="fa-solid fa-plus"></i></button>
</div>
</div>

<div class="card unit">
<h3><i class="fa-solid fa-ruler-combined"></i> Unit Calculator</h3>

<div style="display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:12px;">
  <button type="button" onclick="type.value='length';loadUnits()"><i class="fa-solid fa-ruler"></i> Length</button>
  <button type="button" onclick="type.value='mass';loadUnits()"><i class="fa-solid fa-weight-hanging"></i> Mass</button>
  <button type="button" onclick="type.value='time';loadUnits()"><i class="fa-solid fa-clock"></i> Time</button>
  <button type="button" onclick="type.value='temp';loadUnits()"><i class="fa-solid fa-temperature-half"></i> Temp</button>
  <button type="button" onclick="type.value='speed';loadUnits()"><i class="fa-solid fa-gauge-high"></i> Speed</button>
  <button type="button" onclick="type.value='area';loadUnits()"><i class="fa-solid fa-draw-polygon"></i> Area</button>
  <button type="button" onclick="type.value='volume';loadUnits()" style="grid-column:span 2;"><i class="fa-solid fa-cube"></i> Volume</button>
</div>

<select id="type" style="display:none">
  <option value="length">length</option>
  <option value="mass">mass</option>
  <option value="time">time</option>
  <option value="temp">temp</option>
  <option value="speed">speed</option>
  <option value="area">area</option>
  <option value="volume">volume</option>
</select>
<input type="number" id="val" placeholder="Enter value">
<select id="from"></select>
<select id="to"></select>
<input type="text" id="out" placeholder="Result" disabled>
<button class="equal" style="width:100%" onclick="convert()"><i class="fa-solid fa-rotate"></i> Convert</button>

</div>

</div>

<script>
const d = document.getElementById('display');
const type = document.getElementById('type');
const val = document.getElementById('val');
const from = document.getElementById('from');
const to = document.getElementById('to');
const out = document.getElementById('out');

// Calculator functions
function add(v){ d.value += v; }
function clr(){ d.value = ''; }
function del(){ d.value = d.value.slice(0,-1); }
function calc(){
  if(!d.value) return;
  try{ d.value = eval(d.value); }
  catch(e){ d.value = 'Error'; }
}catch{ d.value = 'Error'; } }

// Unit data
const units = {
  length:{Meter:1,Kilometer:1000,Centimeter:0.01,Millimeter:0.001},
  mass:{Gram:1,Kilogram:1000,Milligram:0.001,Tonne:1000000},
  time:{Second:1,Minute:60,Hour:3600,Day:86400},
  speed:{"m/s":1,"km/h":0.277778},
  area:{"sq m":1,"sq km":1000000,"sq cm":0.0001},
  volume:{Liter:1,Milliliter:0.001,"Cubic Meter":1000},
  temp:{C:1,F:1,K:1}
};

function loadUnits(){
  from.innerHTML = '';
  to.innerHTML = '';
  if(!type.value) return;
  for(let u in units[type.value]){
    from.innerHTML += `<option value="${u}">${u}</option>`;
    to.innerHTML += `<option value="${u}">${u}</option>`;
  }
}

function convert(){
  const v = parseFloat(val.value);
  if(isNaN(v) || !type.value) return;
  const t = type.value;

  if(t === 'temp'){
    let res = v;
    if(from.value==='C' && to.value==='F') res = (v*9/5)+32;
    else if(from.value==='F' && to.value==='C') res = (v-32)*5/9;
    else if(from.value==='C' && to.value==='K') res = v+273.15;
    else if(from.value==='K' && to.value==='C') res = v-273.15;
    else if(from.value==='F' && to.value==='K') res = (v-32)*5/9+273.15;
    else if(from.value==='K' && to.value==='F') res = (v-273.15)*9/5+32;
    out.value = res.toFixed(3);
    return;
  }

  out.value = (v * units[t][from.value] / units[t][to.value]).toFixed(3);
}
// init default
window.addEventListener('load',()=>{ type.value='length'; loadUnits(); });
</script>

</body>
</html>
