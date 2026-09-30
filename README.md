<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<title>Crash Game</title>

<style>
*{
  box-sizing:border-box;
  margin:0;
  padding:0;
}

body{
  background:#071019;
  color:#fff;
  font-family:Tahoma,Arial,sans-serif;
  min-height:100vh;
}

/* HEADER */
.top{
  height:58px;
  background:#0b1620;
  border-bottom:1px solid #1b2a36;
  display:flex;
  align-items:center;
  justify-content:space-between;
  padding:0 12px;
}

.logo{
  font-size:19px;
  font-weight:bold;
}

.logo span{
  color:#19e681;
}

.wallet{
  background:#111f2b;
  border:1px solid #263b49;
  border-radius:10px;
  padding:8px 12px;
  color:#45ed98;
  font-size:13px;
}

/* MAIN */
.container{
  width:100%;
  max-width:1000px;
  margin:auto;
  padding:10px;
}

/* GAME */
.game{
  position:relative;
  height:470px;
  overflow:hidden;
  border-radius:13px;
  background:
    radial-gradient(circle at 55% 55%,rgba(27,111,76,.18),transparent 35%),
    linear-gradient(145deg,#07131d,#050a0f);
  border:1px solid #1d303d;
}

/* grid */
.game:before{
  content:"";
  position:absolute;
  inset:0;
  opacity:.25;
  background-image:
    linear-gradient(#20333f 1px,transparent 1px),
    linear-gradient(90deg,#20333f 1px,transparent 1px);
  background-size:55px 55px;
  transform:perspective(500px) rotateX(58deg) scale(1.8);
  transform-origin:bottom;
}

/* glow */
.glow{
  position:absolute;
  width:420px;
  height:420px;
  border-radius:50%;
  background:rgba(20,226,126,.06);
  filter:blur(50px);
  left:25%;
  top:15%;
}

/* multiplier */
.multiplier{
  position:absolute;
  z-index:10;
  top:40%;
  left:50%;
  transform:translate(-50%,-50%);
  font-size:62px;
  font-weight:900;
  letter-spacing:-2px;
  text-shadow:0 0 25px rgba(43,255,143,.28);
}

/* status */
.status{
  position:absolute;
  z-index:10;
  top:20px;
  width:100%;
  text-align:center;
  color:#80919d;
  font-size:12px;
}

/* flight line */
.flight{
  position:absolute;
  z-index:4;
  width:150%;
  height:4px;
  left:-20%;
  bottom:35px;
  background:#20e681;
  box-shadow:
    0 0 8px #20e681,
    0 0 25px rgba(32,230,129,.5);
  transform:rotate(-18deg);
  transform-origin:left center;
}

/* rocket */
.rocket{
  position:absolute;
  z-index:8;
  left:5%;
  bottom:50px;
  font-size:43px;
  transform:rotate(-18deg);
  filter:drop-shadow(0 0 9px rgba(255,190,60,.7));
  transition:left .05s linear,bottom .05s linear;
}

/* crash */
.crash{
  position:absolute;
  z-index:20;
  inset:0;
  display:none;
  align-items:center;
  justify-content:center;
  flex-direction:column;
  background:rgba(130,0,0,.12);
}

.crash.show{
  display:flex;
}

.crash strong{
  font-size:54px;
  color:#ff4545;
}

.crash small{
  margin-top:8px;
  color:#aeb8bf;
}

/* controls */
.controls{
  display:grid;
  grid-template-columns:1fr 1fr;
  gap:10px;
  margin-top:10px;
}

.panel{
  background:#0b1721;
  border:1px solid #1c303c;
  border-radius:12px;
  padding:12px;
}

.input-title{
  color:#8d9aa4;
  font-size:11px;
  margin-bottom:6px;
}

.bet-input{
  width:100%;
  height:45px;
  border-radius:9px;
  border:1px solid #263b48;
  background:#071019;
  color:#fff;
  padding:0 12px;
  font-size:16px;
  outline:none;
}

.quick{
  display:flex;
  gap:5px;
  margin-top:6px;
}

.quick button{
  flex:1;
  border:0;
  background:#132531;
  color:#9eabb3;
  padding:6px;
  border-radius:6px;
  font-size:10px;
}

/* buttons */
.buttons{
  display:grid;
  grid-template-columns:1fr 1fr;
  gap:8px;
  margin-top:10px;
}

button{
  border:0;
  cursor:pointer;
}

.start{
  height:48px;
  background:#16d878;
  color:#03150d;
  border-radius:9px;
  font-size:16px;
  font-weight:bold;
}

.cash{
  height:48px;
  background:#f2a900;
  color:#111;
  border-radius:9px;
  font-size:16px;
  font-weight:bold;
}

.cash:disabled,
.start:disabled{
  opacity:.35;
  cursor:not-allowed;
}

/* bottom */
.bottom{
  margin-top:10px;
  background:#0a151e;
  border:1px solid #1b2c38;
  border-radius:12px;
  overflow:hidden;
}

.tabs{
  height:45px;
  display:flex;
  border-bottom:1px solid #1b2c38;
}

.tab{
  flex:1;
  display:flex;
  justify-content:center;
  align-items:center;
  font-size:12px;
  color:#7e8d97;
}

.tab.active{
  color:#35ed91;
  border-bottom:2px solid #35ed91;
}

.rows{
  max-height:220px;
  overflow:auto;
}

.row{
  display:grid;
  grid-template-columns:1.5fr .8fr .8fr .8fr;
  align-items:center;
  padding:10px;
  border-bottom:1px solid #14242e;
  font-size:11px;
}

.row.head{
  color:#657681;
  font-size:10px;
}

.green{
  color:#32e98b;
}

.red{
  color:#ff5050;
}

.user{
  display:flex;
  align-items:center;
  gap:5px;
}

.avatar{
  width:22px;
  height:22px;
  display:flex;
  align-items:center;
  justify-content:center;
  background:#152833;
  border-radius:50%;
  font-size:10px;
}

/* message */
.message{
  height:28px;
  text-align:center;
  font-size:11px;
  color:#8d9ba4;
  padding-top:8px;
}

/* mobile */
@media(max-width:600px){

  .game{
    height:390px;
  }

  .multiplier{
    font-size:48px;
  }

  .rocket{
    font-size:35px;
  }

  .container{
    padding:7px;
  }

}

</style>
</head>

<body>

<div class="top">

  <div class="logo">
    CRASH <span>GAME</span>
  </div>

  <div class="wallet">
    موجودی $<b id="balance">1.00</b>
  </div>

</div>


<div class="container">

  <div class="game">

    <div class="glow"></div>

    <div class="status" id="status">
      برای شروع بازی آماده‌اید؟
    </div>

    <div class="multiplier" id="multiplier">
      1.00x
    </div>

    <div class="flight" id="flight"></div>

    <div class="rocket" id="rocket">
      🚀
    </div>

    <div class="crash" id="crash">

      <strong id="crashNumber">
        1.00x
      </strong>

      <small>
        انفجار
      </small>

    </div>

  </div>


  <div class="controls">

    <div class="panel">

      <div class="input-title">
        مبلغ شرط
      </div>

      <input
        id="bet"
        class="bet-input"
        type="number"
        min="0.01"
        max="1"
        step="0.01"
        value="1.00"
      >

      <div class="quick">

        <button onclick="setBet(.10)">
          $0.10
        </button>

        <button onclick="setBet(.25)">
          $0.25
        </button>

        <button onclick="setBet(.50)">
          $0.50
        </button>

        <button onclick="setBet(1)">
          $1
        </button>

      </div>

    </div>


    <div class="panel">

      <div class="input-title">
        عملیات
      </div>

      <div class="buttons">

        <button
          class="start"
          id="start"
          onclick="startGame()"
        >
          شروع
        </button>

        <button
          class="cash"
          id="cash"
          onclick="cashout()"
          disabled
        >
          برداشت
        </button>

      </div>

    </div>

  </div>


  <div class="message" id="message">
    موجودی اولیه بازی مجازی: $1.00
  </div>


  <div class="bottom">

    <div class="tabs">

      <div class="tab active">
        بازی‌های کاربران
      </div>

      <div class="tab">
        تاریخچه من
      </div>

    </div>


    <div class="rows" id="rows">

      <div class="row head">

        <span>کاربر</span>
        <span>شرط</span>
        <span>ضریب</span>
        <span>نتیجه</span>

      </div>

    </div>

  </div>

</div>


<script>

let balance = 1;

let playing = false;

let bet = 0;

let multiplier = 1;

let crashPoint = 0;

let startTime = 0;

let timer = null;

const balanceEl =
document.getElementById("balance");

const multiplierEl =
document.getElementById("multiplier");

const rocket =
document.getElementById("rocket");

const flight =
document.getElementById("flight");

const crash =
document.getElementById("crash");

const crashNumber =
document.getElementById("crashNumber");

const status =
document.getElementById("status");

const message =
document.getElementById("message");

const rows =
document.getElementById("rows");

const start =
document.getElementById("start");

const cash =
document.getElementById("cash");

const betInput =
document.getElementById("bet");


function updateBalance(){

  balanceEl.textContent =
    balance.toFixed(2);

}


function setBet(value){

  if(value > balance){
    value = balance;
  }

  betInput.value =
    value.toFixed(2);

}


function createCrash(){

  const r = Math.random();

  if(r < .50){

    return 1.10 +
      Math.random()*1.20;

  }

  if(r < .82){

    return 2.30 +
      Math.random()*2.70;

  }

  if(r < .96){

    return 5 +
      Math.random()*10;

  }

  return 15 +
    Math.random()*35;

}


function resetVisual(){

  crash.classList.remove("show");

  multiplier = 1;

  multiplierEl.textContent =
    "1.00x";

  multiplierEl.style.color =
    "#fff";

  rocket.style.left =
    "5%";

  rocket.style.bottom =
    "50px";

  flight.style.background =
    "#20e681";

  flight.style.boxShadow =
    "0 0 8px #20e681,0 0 25px rgba(32,230,129,.5)";

}


function startGame(){

  if(playing) return;

  bet =
    Number(betInput.value);

  if(!bet || bet <= 0){

    message.textContent =
      "مبلغ را وارد کنید.";

    return;

  }

  if(bet > balance){

    message.textContent =
      "موجودی کافی نیست.";

    return;

  }

  balance -= bet;

  updateBalance();

  playing = true;

  start.disabled = true;

  cash.disabled = false;

  betInput.disabled = true;

  crashPoint =
    Number(createCrash().toFixed(2));

  startTime =
    Date.now();

  resetVisual();

  status.textContent =
    "🚀 در حال پرواز...";

  message.textContent =
    "در هر لحظه می‌توانید برداشت کنید.";

  timer =
    setInterval(updateGame,35);

}


function updateGame(){

  if(!playing) return;

  const seconds =
    (Date.now()-startTime)/1000;

  multiplier =
    Math.pow(1.18,seconds);

  multiplier =
    Number(multiplier.toFixed(2));

  if(multiplier >= crashPoint){

    multiplier =
      crashPoint;

    doCrash();

    return;

  }

  multiplierEl.textContent =
    multiplier.toFixed(2)+"x";


  /* رنگ */

  if(multiplier < 2){

    multiplierEl.style.color =
      "#35ed91";

    flight.style.background =
      "#20e681";

  }
  else if(multiplier < 4){

    multiplierEl.style.color =
      "#ffd23f";

    flight.style.background =
      "#ffd23f";

  }
  else{

    multiplierEl.style.color =
      "#ff4c4c";

    flight.style.background =
      "#ff3c3c";

  }


  /* حرکت */

  const progress =
    Math.min(
      .94,
      Math.log(multiplier) /
      Math.log(20)
    );

  const left =
    5 + progress*88;

  const bottom =
    50 + progress*300;

  rocket.style.left =
    left+"%";

  rocket.style.bottom =
    Math.min(bottom,350)+"px";

}


function cashout(){

  if(!playing) return;

  const win =
    bet*multiplier;

  balance += win;

  updateBalance();

  addRow(
    "شما",
    bet,
    multiplier,
    "+"+win.toFixed(2),
    true
  );

  message.textContent =
    "🎉 برداشت موفق: $"+
    win.toFixed(2);

  message.style.color =
    "#35ed91";

  finish();

}


function doCrash(){

  if(!playing) return;

  multiplierEl.textContent =
    multiplier.toFixed(2)+"x";

  multiplierEl.style.color =
    "#ff4444";

  flight.style.background =
    "#ff3333";

  crashNumber.textContent =
    multiplier.toFixed(2)+"x";

  crash.classList.add("show");

  status.textContent =
    "💥 انفجار!";

  message.textContent =
    "این دور باختید.";

  message.style.color =
    "#ff5050";

  addRow(
    "شما",
    bet,
    multiplier,
    "باخت",
    false
  );

  finish();

}


function finish(){

  playing = false;

  clearInterval(timer);

  timer = null;

  start.disabled = false;

  cash.disabled = true;

  betInput.disabled = false;

}


/* کاربران نمایشی */

const users = [
  "Player_482",
  "CryptoFox",
  "Moon_77",
  "LuckyDog",
  "RocketX",
  "Trader_19",
  "User_351",
  "BTC_Master"
];


function addRow(
  username,
  amount,
  mult,
  result,
  win
){

  const row =
    document.createElement("div");

  row.className =
    "row";

  row.innerHTML = `

    <span class="user">

      <span class="avatar">
        👤
      </span>

      ${username}

    </span>

    <span>
      $${amount.toFixed(2)}
    </span>

    <span>
      ${mult.toFixed(2)}x
    </span>

    <span class="${win ? "green":"red"}">
      ${result}
    </span>

  `;

  rows.insertBefore(
    row,
    rows.children[1]
  );

  while(rows.children.length > 14){

    rows.removeChild(
      rows.lastChild
    );

  }

}


function demoUser(){

  const user =
    users[
      Math.floor(
        Math.random()*users.length
      )
    ];

  const amount =
    Number(
      (0.05+
      Math.random()*.95)
      .toFixed(2)
    );

  const win =
    Math.random() > .43;

  const mult =
    win
      ? 1.2+Math.random()*7
      : 1.1+Math.random()*2;

  const result =
    win
      ? "+"+(amount*mult).toFixed(2)
      : "باخت";

  addRow(
    user,
    amount,
    mult,
    result,
    win
  );

}


for(let i=0;i<7;i++){

  setTimeout(
    demoUser,
    i*300
  );

}

setInterval(
  demoUser,
  1800
);

updateBalance();

</script>

</body>
</html>
