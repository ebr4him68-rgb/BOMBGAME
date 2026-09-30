# BOMBGAME
`
<!-- ==============================
     BACKGROUND + GLOBAL GAME PANEL
     این بخش را به نسخه بازی قبلی اضافه کن
================================ -->

<style>

/* پس‌زمینه زن بزرگسال */
body{
    background:
        linear-gradient(
            rgba(3,7,18,.72),
            rgba(3,7,18,.88)
        ),
        url("https://images.unsplash.com/photo-1534528741775-53994a69daeb?auto=format&fit=crop&w=1200&q=85")
        center/cover fixed no-repeat;

    background-color:#050811;
}


/* پنل بازی‌های کاربران */
.global-games{
    width:100%;
    max-width:600px;
    margin:12px auto 0;
    background:rgba(8,15,28,.94);
    border:1px solid #263650;
    border-radius:15px;
    overflow:hidden;
    box-shadow:0 10px 35px rgba(0,0,0,.4);
}

.global-title{
    padding:11px 14px;
    background:#111b2d;
    display:flex;
    justify-content:space-between;
    align-items:center;
    font-size:14px;
    font-weight:bold;
}

.live-users{
    color:#4ade80;
    font-size:11px;
}

.global-list{
    max-height:145px;
    overflow-y:auto;
}

.global-row{
    display:grid;
    grid-template-columns:
        1.3fr .8fr .7fr .8fr;

    gap:5px;
    align-items:center;

    padding:9px 11px;

    border-top:1px solid #1e293b;

    font-size:11px;
}

.global-user{
    overflow:hidden;
    text-overflow:ellipsis;
    white-space:nowrap;
}

.global-multiplier{
    font-weight:bold;
}

.global-win{
    color:#4ade80;
}

.global-loss{
    color:#f87171;
}

.global-playing{
    color:#facc15;
}

.global-head{
    color:#64748b;
    font-size:10px;
    background:#0b1220;
}

.global-dot{
    display:inline-block;
    width:6px;
    height:6px;
    border-radius:50%;
    margin-left:4px;
    background:#22c55e;
    box-shadow:0 0 7px #22c55e;
}

</style>


<!-- =================================
     پنل پایین بازی
================================= -->

<div class="global-games">

    <div class="global-title">

        <span>
            🎮 بازی‌های کاربران
        </span>

        <span class="live-users">
            <span class="global-dot"></span>
            آنلاین
        </span>

    </div>


    <div class="global-list">

        <!-- عنوان -->
        <div class="global-row global-head">

            <span>کاربر</span>
            <span>مبلغ</span>
            <span>ضریب</span>
            <span>نتیجه</span>

        </div>


        <div id="globalGameList"></div>

    </div>

</div>


<script>

/* =================================
   نام‌های نمایشی نمونه
================================= */

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


/* =================================
   ساخت بازی نمونه
================================= */

function createDemoGame(){

    const user =
        demoUsers[
            Math.floor(
                Math.random() *
                demoUsers.length
            )
        ];


    const amount =
        (
            Math.random() * 0.95 +
            0.05
        ).toFixed(2);


    const multiplier =
        (
            1 +
            Math.random() * 5
        ).toFixed(2);


    const win =
        Math.random() > .48;


    return {

        user:user,

        amount:amount,

        multiplier:multiplier,

        result:
            win
            ? "برد"
            : "باخت",

        status:
            win
            ? "win"
            : "loss"

    };

}


/* =================================
   اضافه کردن بازی به پنل
================================= */

function addGlobalGame(game){

    const list =
        document.getElementById(
            "globalGameList"
        );


    const row =
        document.createElement("div");


    row.className =
        "global-row";


    const statusClass =
        game.status === "win"
        ? "global-win"
        : "global-loss";


    row.innerHTML = `

        <span class="global-user">
            ${game.user}
        </span>

        <span>
            $${game.amount}
        </span>

        <span class="global-multiplier">
            ${game.multiplier}x
        </span>

        <span class="${statusClass}">
            ${game.result}
        </span>

    `;


    list.prepend(row);


    /*
       بیشتر از 12 نتیجه نمایش نده
    */

    while(list.children.length > 12){

        list.removeChild(
            list.lastElementChild
        );

    }

}


/* =================================
   بازی‌های اولیه نمایشی
================================= */

for(let i = 0; i < 8; i++){

    addGlobalGame(
        createDemoGame()
    );

}


/* =================================
   اضافه شدن بازی‌های نمایشی جدید
================================= */

setInterval(

    function(){

        addGlobalGame(
            createDemoGame()
        );

    },

    2500

);


/* =================================
   ثبت بازی واقعی کاربر در پنل
================================ */

const oldAddHistory =
    window.addHistory;


/*
   تابع بازی قبلی را دوباره تعریف می‌کنیم
   تا نتیجه کاربر هم در پنل دیده شود.
*/

window.addHistory =
function(
    bet,
    multiplier,
    result,
    status
){

    /*
       اجرای تاریخچه اصلی
       اگر تابع اصلی موجود باشد
    */

    if(typeof oldAddHistory === "function"){

        oldAddHistory(
            bet,
            multiplier,
            result,
            status
        );

    }


    /*
       اضافه کردن نتیجه کاربر
    */

    addGlobalGame({

        user:"شما",

        amount:
            Number(bet).toFixed(2),

        multiplier:
            Number(multiplier).toFixed(2),

        result:
            status === "برد"
            ? "برد"
            : "باخت",

        status:
            status === "برد"
            ? "win"
            : "loss"

    });

};

</script>
```

