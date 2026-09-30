<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<title>بازی انفجار</title>

<style>
*{
  box-sizing:border-box;
  margin:0;
  padding:0;
}

body{
  font-family:Tahoma,Arial,sans-serif;
  min-height:100vh;
  color:#fff;
  background:
    linear-gradient(rgba(0,0,0,.72),rgba(0,0,0,.88)),
    url("https://images.unsplash.com/photo-1534528741775-53994a69daeb?auto=format&fit=crop&w=1200&q=85")
    center/cover fixed no-repeat;
  padding:12px;
}

.game{
  width:100%;
  max-width:600px;
  min-height:50vh;
  margin:0 auto;
  background:rgba(8,12,20,.92);
  border:1px solid rgba(255,255,255,.12);
  border-radius:18px;
  padding:14px;
  box-shadow:0 15px 45px rgba(0,0,0,.6);
}

.header{
  display:flex;
  justify-content:space-between;
  align-items:center;
  margin-bottom:12px;
}

.title{
  font-size:22px;
  font-weight:bold;
}

.balance{
  background:#101c2b;
  padding:9px 13px;
  border-radius:12px;
  color:#4cff9a;
  font-weight:bold;
}

.chart{
  position:relative;
  height:330px;
  overflow:hidden;
  border-radius:15px;
  background:
    linear-gradient(rgba(15,25,40,.55),rgba(2,6,12,.95)),
    repeating-linear-gradient(
      0deg,
      transparent 0,
      transparent 39px,
      rgba(255,255,255,.04) 40px
    );
  border:1px solid rgba(255,255,255,.08);
}

.multiplier{
  position:absolute;
  left:50%;
  top:42%;
  transform:translate(-50%,-50%);
  font-size:42px;
  font-weight:bold;
  text-shadow:0 0 25px rgba(0,255,100,.5);
  z-index:5;
}

.status{
  position:absolute;
  top:12px;
  left:12px;
  right:12px;
  text-align:center;
  font-size:14px;
  color:#aaa;
  z-index:6;
}

.flight-line{
  position:absolute;
  width:140%;
  height:5px;
  left:-20%;
  bottom:25px;
  background:#20e878;
  transform:rotate(-12deg);
  transform-origin:left center;
  box-shadow:0 0 14px #20e878;
  transition:.15s;
}

.rocket{
  position:absolute;
  font-size:38px;
  left:30px;
  bottom:42px;
  z-index:7;
  transform:rotate(-18deg);
  transition:left .08s linear,bottom .08s linear;
  filter:drop-shadow(0 0 10px rgba(255,180,0,.7));
}

.controls{
  margin-top:12px;
  display:grid;
  grid-template-columns:1fr 1fr;
  gap:8px;
}

.controls button{
  border:0;
  padding:14px;
  border-radius:12px;
  font-size:17px;
  font-weight:bold;
  cursor:pointer;
}

.start{
  background:#16c96b;
  color:#fff;
}

.cashout{
  background:#ff9d00;
  color:#111;
}

.cashout:disabled{
  opacity:.4;
  cursor:not-allowed;
}

.betbox{
  margin-top:10px;
  display:flex;
  align-items:center;
  gap:8px;
}

.betbox label{
  color:#aaa;
  font-size:14px;
}

.betbox input{
  flex:1;
  min-width:0;
  background:#0c1522;
  border:1px solid #26374b;
  color:#fff;
  border-radius:10px;
  padding:11px;
  outline:none;
}

.message{
  text-align:center;
  margin-top:10px;
  min-height:22px;
  font-size:14px;
}

.history{
  margin-top:14px;
  background:rgba(9,15,24,.9);
  border-radius:15px;
  padding:12px;
}

.history-title{
  font-size:16px;
  font-weight:bold;
  margin-bottom:8px;
}

.history-list{
  max-height:180px;
  overflow-y:auto;
}

.history-row{
  display:grid;
  grid-template-columns:1fr .8fr .8fr;
  gap:5px;
  padding:9px 5px;
  border-bottom:1px solid rgba(255,255,255,.06);
  font-size:12px;
}

.win{
  color:#39ed91;
}

.loss{
  color:#ff5151;
}

/* پنل بازی‌های کاربران */
.global-games{
  width:100%;
  max-width:600px;
  margin:12px auto 0;
  background:rgba(7,12,20,.94);
  border:1px solid rgba(255,255,255,.1);
  border-radius:16px;
  padding:12px;
  box-shadow:0 10px 30px rgba(0,0,0,.5);
}

.global-title{
  display:flex;
  justify-content:space-between;
  align-items:center;
  margin-bottom:9px;
  font-size:15px;
  font-weight:bold;
}

.live{
  color:#42ff92;
  font-size:11px;
}

.global-list{
  max-height:190px;
  overflow-y:auto;
}

.global-row{
  display:grid;
  grid-template-columns:1.3fr .8fr .7fr .7fr;
  align-items:center;
  gap:5px;
  padding:8px 4px;
  border-bottom:1px solid rgba(255,255,255,.06);
  font-size:11px;
}

.global-head{
  color:#7f8c9b;
  font-size:10px;
}

.global-win{
  color:#38ee91;
}

.global-loss{
  color:#ff5050;
}

.dot{
  width:7px;
  height:7px;
  display:inline-block;
  border-radius:50%;
  background:#39ed91;
  margin-left:4px;
  box-shadow:0 0 8px #39ed91;
}

.footer{
  text-align:center;
  color:#6f7b89;
  font-size:10px;
  margin-top:10px;
}
</style>
</head>

<body>

<div class="game">

  <div class="header">
    <div class="title">🚀 بازی انفجار</div>
    <div class="balance">
      موجودی: $<span id="balance">1.00</span>
    </div>
  </div>

  <div class="chart" id="chart">

    <div class="status" id="status">
      برای شروع بازی دکمه شروع را بزنید
    </div>

    <div class="multiplier" id="multiplier">
      1.00x
    </div>

    <div class="flight-line" id="flightLine"></div>

    <div class="rocket" id="rocket">
      🚀
    </div>

  </div>

  <div class="betbox">
    <label>مبلغ بازی:</label>
    <input
      id="bet"
      type="number"
      min="0.01"
      max="1"
      step="0.01"
      value="1.00"
    >
  </div>

  <div class="controls">
    <button class="start" id="startBtn">
      ▶ شروع بازی
    </button>

    <button class="cashout" id="cashoutBtn" disabled>
      💰 توقف / برداشت
    </button>
  </div>

  <div class="message" id="message"></div>

  <div class="history">
    <div class="history-title">
      📜 تاریخچه بازی‌های شما
    </div>

    <div class="history-list" id="historyList"></div>
  </div>

</div>


<!-- پنل تمام کاربران -->
<div class="global-games">

  <div class="global-title">
    <span>👥 بازی‌های کاربران</span>
    <span class="live">
      <span class="dot"></span>
      زنده
    </span>
  </div>

  <div class="global-list" id="globalList">

    <div class="global-row global-head">
      <span>کاربر</span>
      <span>مبلغ</span>
      <span>ضریب</span>
      <span>نتیجه</span>
    </div>

  </div>

  <div class="footer">
    نتایج کاربران در این نسخه نمایشی هستند.
  </div>

</div>


<script>

/* =========================
   تنظیمات بازی
========================= */

let balance = 1.00;

let playing = false;

let startTime = 0;

let timer = null;

let multiplier = 1.00;

let crashPoint = 0;

let currentBet = 0;

let history = [];

const balanceEl = document.getElementById("balance");
const multiplierEl = document.getElementById("multiplier");
const rocketEl = document.getElementById("rocket");
const flightLineEl = document.getElementById("flightLine");
const statusEl = document.getElementById("status");
const messageEl = document.getElementById("message");

const startBtn = document.getElementById("startBtn");
const cashoutBtn = document.getElementById("cashoutBtn");

const betInput = document.getElementById("bet");

const chart = document.getElementById("chart");

const historyList = document.getElementById("historyList");

const globalList = document.getElementById("globalList");


/* =========================
   موجودی
========================= */

function updateBalance(){

  balanceEl.textContent = balance.toFixed(2);

}


/* =========================
   تولید ضریب انفجار
========================= */

function generateCrashPoint(){

  const random = Math.random();

  let value;

  if(random < 0.55){

    value = 1.10 + Math.random() * 1.20;

  }else if(random < 0.85){

    value = 2.30 + Math.random() * 2.70;

  }else{

    value = 5.00 + Math.random() * 5.00;

  }

  return Number(value.toFixed(2));

}


/* =========================
   شروع بازی
========================= */

function startGame(){

  if(playing) return;

  currentBet = Number(betInput.value);

  if(!Number.isFinite(currentBet) || currentBet <= 0){

    messageEl.textContent = "مبلغ بازی را درست وارد کنید.";
    messageEl.style.color = "#ff5555";
    return;

  }

  if(currentBet > balance){

    messageEl.textContent = "موجودی کافی نیست.";
    messageEl.style.color = "#ff5555";
    return;

  }

  balance -= currentBet;

  updateBalance();

  playing = true;

  startTime = Date.now();

  multiplier = 1.00;

  crashPoint = generateCrashPoint();

  startBtn.disabled = true;

  cashoutBtn.disabled = false;

  betInput.disabled = true;

  messageEl.textContent = "";

  statusEl.textContent = "🚀 بازی در حال حرکت است...";

  multiplierEl.textContent = "1.00x";

  multiplierEl.style.color = "#35ef8a";

  rocketEl.style.left = "30px";

  rocketEl.style.bottom = "42px";

  flightLineEl.style.background = "#20e878";

  flightLineEl.style.boxShadow = "0 0 14px #20e878";

  timer = setInterval(updateGame, 50);

}


/* =========================
   اجرای بازی
========================= */

function updateGame(){

  if(!playing) return;

  const elapsed = (Date.now() - startTime) / 1000;

  multiplier = Math.pow(1.18, elapsed);

  multiplier = Number(multiplier.toFixed(2));

  if(multiplier >= crashPoint){

    multiplier = crashPoint;

    crashGame();

    return;

  }

  multiplierEl.textContent = multiplier.toFixed(2) + "x";


  /* تغییر رنگ */

  if(multiplier < 1.80){

    multiplierEl.style.color = "#38ef8d";

    flightLineEl.style.background = "#20e878";

    flightLineEl.style.boxShadow = "0 0 14px #20e878";

  }
  else if(multiplier < 3){

    multiplierEl.style.color = "#ffd43b";

    flightLineEl.style.background = "#ffd43b";

    flightLineEl.style.boxShadow = "0 0 14px #ffd43b";

  }
  else{

    multiplierEl.style.color = "#ff4444";

    flightLineEl.style.background = "#ff3030";

    flightLineEl.style.boxShadow = "0 0 18px #ff3030";

  }


  /* حرکت موشک */

  const maxLeft = chart.clientWidth - 65;

  const progress = Math.min(
    0.92,
    Math.log(multiplier) / Math.log(10)
  );

  const left = 30 + (maxLeft - 30) * progress;

  const maxBottom = chart.clientHeight - 80;

  const bottom = 42 + maxBottom * progress;

  rocketEl.style.left = left + "px";

  rocketEl.style.bottom = Math.min(
    bottom,
    chart.clientHeight - 65
  ) + "px";

}


/* =========================
   برداشت دستی
========================= */

function cashout(){

  if(!playing) return;

  const winAmount = currentBet * multiplier;

  balance += winAmount;

  updateBalance();

  const result = {

    type:"برد",

    bet:currentBet,

    multiplier:multiplier,

    amount:winAmount,

    time:new Date().toLocaleTimeString("fa-IR")

  };

  saveHistory(result);

  messageEl.textContent =
    "🎉 بردید $" + winAmount.toFixed(2);

  messageEl.style.color = "#39ed91";

  finishGame();

}


/* =========================
   انفجار
========================= */

function crashGame(){

  if(!playing) return;

  const result = {

    type:"باخت",

    bet:currentBet,

    multiplier:crashPoint,

    amount:0,

    time:new Date().toLocaleTimeString("fa-IR")

  };

  saveHistory(result);

  messageEl.textContent =
    "💥 انفجار در " + crashPoint.toFixed(2) + "x";

  messageEl.style.color = "#ff4d4d";

  multiplierEl.textContent =
    crashPoint.toFixed(2) + "x 💥";

  multiplierEl.style.color = "#ff3030";

  statusEl.textContent = "💥 انفجار!";

  finishGame();

}


/* =========================
   پایان بازی
========================= */

function finishGame(){

  playing = false;

  clearInterval(timer);

  timer = null;

  startBtn.disabled = false;

  cashoutBtn.disabled = true;

  betInput.disabled = false;

}


/* =========================
   تاریخچه شخصی
========================= */

function saveHistory(result){

  history.unshift(result);

  history = history.slice(0,30);

  localStorage.setItem(
    "virtualCrashHistory",
    JSON.stringify(history)
  );

  renderHistory();

  addGlobalUserResult(result);

}


/* =========================
   نمایش تاریخچه
========================= */

function renderHistory(){

  historyList.innerHTML = "";

  history.forEach((item,index)=>{

    const row = document.createElement("div");

    row.className = "history-row";

    row.innerHTML = `

      <span>
        ${item.time}
      </span>

      <span>
        ${item.multiplier.toFixed(2)}x
      </span>

      <span class="${item.type === "برد" ? "win" : "loss"}">
        ${item.type === "برد"
          ? "+$" + item.amount.toFixed(2)
          : "باخت"}
      </span>

    `;

    historyList.appendChild(row);

  });

}


/* =========================
   نتایج کاربران
========================= */

const demoUsers = [

  "Player_482",
  "CryptoFox",
  "Moon_77",
  "LuckyDog",
  "Trader_19",
  "RocketX",
  "User_351",
  "BTC_Master"

];


function addGlobalUserResult(result){

  const row = document.createElement("div");

  row.className = "global-row";

  const user =
    "شما";

  row.innerHTML = `

    <span>
      <span class="dot"></span>
      ${user}
    </span>

    <span>
      $${result.bet.toFixed(2)}
    </span>

    <span>
      ${result.multiplier.toFixed(2)}x
    </span>

    <span class="${
      result.type === "برد"
      ? "global-win"
      : "global-loss"
    }">

      ${
        result.type === "برد"
        ? "+" + result.amount.toFixed(2)
        : "باخت"
      }

    </span>

  `;

  globalList.insertBefore(
    row,
    globalList.children[1]
  );

}


/* =========================
   کاربران نمایشی
========================= */

function addDemoUser(){

  const user =
    demoUsers[
      Math.floor(Math.random()*demoUsers.length)
    ];

  const bet =
    (Math.floor(Math.random()*10)+1)/10;

  const crashed =
    Math.random() < .45;

  const mult =
    crashed
      ? 1.10 + Math.random()*1.7
      : 1.50 + Math.random()*5.5;

  const result =
    crashed
      ? "باخت"
      : "برد";

  const amount =
    result === "برد"
      ? bet * mult
      : 0;

  const row = document.createElement("div");

  row.className = "global-row";

  row.innerHTML = `

    <span>
      <span class="dot"></span>
      ${user}
    </span>

    <span>
      $${bet.toFixed(2)}
    </span>

    <span>
      ${mult.toFixed(2)}x
    </span>

    <span class="${
      result === "برد"
      ? "global-win"
      : "global-loss"
    }">

      ${
        result === "برد"
        ? "+$" + amount.toFixed(2)
        : "باخت"
      }

    </span>

  `;

  globalList.insertBefore(
    row,
    globalList.children[1]
  );


  /* نگه داشتن پنل کوچک */

  while(globalList.children.length > 16){

    globalList.removeChild(
      globalList.lastChild
    );

  }

}


/* =========================
   رویدادها
========================= */

startBtn.addEventListener(
  "click",
  startGame
);

cashoutBtn.addEventListener(
  "click",
  cashout
);


/* جلوگیری از شرط بیشتر از موجودی */

betInput.addEventListener(
  "input",
  function(){

    if(Number(this.value) > balance){

      this.value = balance.toFixed(2);

    }

  }
);


/* =========================
   بارگذاری تاریخچه
========================= */

try{

  const saved =
    localStorage.getItem(
      "virtualCrashHistory"
    );

  if(saved){

    history = JSON.parse(saved);

    renderHistory();

  }

}catch(e){

  history = [];

}


/* =========================
   شروع نتایج نمایشی کاربران
========================= */

setInterval(
  addDemoUser,
  2500
);


/* چند نتیجه اولیه */

for(let i=0;i<5;i++){

  setTimeout(
    addDemoUser,
    i * 500
  );

}

updateBalance();

</script>

</body>
</html>
