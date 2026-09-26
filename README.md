<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<title>Türkçe Avcısı</title>

<style>
*{
    box-sizing:border-box;
    margin:0;
    padding:0;
    font-family:Arial,sans-serif;
}

html,body{
    min-height:100%;
}

body{
    min-height:100vh;
    background:#050508;
    color:white;
    display:flex;
    justify-content:center;
    align-items:flex-start;
    overflow:auto;
    padding:15px 0;
}

#bg{
    position:fixed;
    inset:0;
    z-index:0;
    pointer-events:none;
}

.game{
    position:relative;
    z-index:2;
    width:92%;
    max-width:420px;
    max-height:96vh;
    overflow-y:auto;
    padding:20px;
    border-radius:18px;
    background:rgba(15,23,42,.9);
    border:2px solid #00f2fe;
    box-shadow:0 0 30px #000;
    text-align:center;
}

h1{
    color:white;
    text-shadow:0 0 12px #00f2fe;
    margin-bottom:5px;
}

.subtitle{
    color:#00ff87;
    font-size:13px;
    margin-bottom:15px;
}

.login{
    padding:14px;
    border-radius:12px;
    background:rgba(255,255,255,.08);
    border:1px solid rgba(0,242,254,.4);
    margin-bottom:15px;
}

.login h3{
    margin-bottom:9px;
}

input{
    width:100%;
    padding:11px;
    border-radius:8px;
    border:1px solid #00f2fe;
    background:#080c14;
    color:white;
    outline:none;
    font-size:14px;
    user-select:text;
}

.login button{
    width:100%;
    margin-top:8px;
    padding:10px;
    border:0;
    border-radius:8px;
    background:white;
    color:#111;
    font-weight:bold;
    cursor:pointer;
}

.info{
    margin-top:8px;
    color:#aaa;
    font-size:11px;
}

.account{
    color:#00ff87;
    font-size:12px;
    word-break:break-all;
}

.logout{
    margin-top:8px;
    padding:6px 12px;
    border:1px solid #ff007f;
    border-radius:6px;
    background:transparent;
    color:#ff007f;
    cursor:pointer;
}

.stats{
    display:flex;
    justify-content:center;
    gap:30px;
    padding:10px;
    margin-bottom:15px;
    background:rgba(255,255,255,.08);
    border-radius:10px;
}

.lives{
    color:#ff007f;
}

.score{
    color:#ffe600;
}

button{
    cursor:pointer;
}

.btn{
    width:100%;
    padding:11px;
    border:0;
    border-radius:9px;
    margin-top:8px;
    font-weight:bold;
    font-size:14px;
}

.easy{
    background:#00b09b;
    color:white;
}

.normal{
    background:#f7b731;
    color:white;
}

.hard{
    background:#eb3b5a;
    color:white;
}

.fast{
    background:#ff007f;
    color:white;
}

.start{
    background:linear-gradient(90deg,#00f2fe,#4facfe);
    color:#000;
}

.shop{
    background:linear-gradient(90deg,#ffe600,#ff007f);
    color:white;
}

.selected{
    outline:2px solid white;
}

.hud{
    display:flex;
    justify-content:space-between;
    padding:10px;
    background:rgba(255,255,255,.08);
    border-radius:8px;
    margin-bottom:12px;
}

.category{
    display:inline-block;
    padding:4px 10px;
    border-radius:20px;
    border:1px solid #ff007f;
    color:#ff007f;
    font-size:11px;
    margin-bottom:8px;
}

.question{
    min-height:65px;
    display:flex;
    justify-content:center;
    align-items:center;
    padding:10px;
    border-left:3px solid #00f2fe;
    background:rgba(0,242,254,.08);
    border-radius:8px;
    margin-bottom:12px;
    font-weight:bold;
    font-size:14px;
}

.options{
    display:flex;
    flex-direction:column;
    gap:8px;
}

.option{
    padding:11px;
    border-radius:8px;
    border:1px solid rgba(0,242,254,.4);
    background:rgba(255,255,255,.1);
    color:white;
    text-align:left;
}

.correct{
    background:rgba(0,255,135,.5)!important;
}

.wrong{
    background:rgba(255,0,127,.5)!important;
}

.shop-item{
    display:flex;
    justify-content:space-between;
    align-items:center;
    padding:12px;
    margin-bottom:8px;
    background:rgba(255,255,255,.08);
    border:1px solid rgba(255,230,0,.4);
    border-radius:9px;
}

.buy{
    padding:7px 12px;
    border:0;
    border-radius:6px;
    background:#00ff87;
    font-weight:bold;
}

.hide{
    display:none!important;
}

/* COMBO */

.combo{
    position:fixed;
    left:50%;
    top:18%;
    transform:translateX(-50%) scale(.7);
    color:#ff8c00;
    font-size:38px;
    font-weight:900;
    text-shadow:
        0 0 8px #ff4500,
        0 0 18px #ff0000,
        0 0 30px #ffe600;
    z-index:20;
    pointer-events:none;
    opacity:0;
}

.combo.show{
    animation:comboPop .8s ease-out;
}

@keyframes comboPop{

    0%{
        opacity:0;
        transform:translateX(-50%) scale(.5);
    }

    30%{
        opacity:1;
        transform:translateX(-50%) scale(1.2);
    }

    70%{
        opacity:1;
        transform:translateX(-50%) scale(1);
    }

    100%{
        opacity:0;
        transform:translateX(-50%) scale(1.1);
    }
}

@media(max-height:650px){

    body{
        padding:8px 0;
    }

    .game{
        max-height:94vh;
        padding:14px;
    }
}
</style>
</head>

<body>

<canvas id="bg"></canvas>

<div id="combo" class="combo"></div>

<div class="game">

<div class="login">

<div id="loginArea">

<h3>📧 Gmail ile Kayıt Ol</h3>

<input
    id="gmail"
    type="email"
    placeholder="ornek@gmail.com"
>

<button onclick="login()">
    Gmail ile Kayıt Ol / Giriş Yap
</button>

<div class="info">
    Gmail ile kayıt olmayanların oyun ilerlemesi kaydedilmez.
</div>

</div>

<div id="accountArea" class="hide">

<div class="account">
    ✅ Giriş yapıldı
    <br>
    <span id="accountEmail"></span>
</div>

<button class="logout" onclick="logout()">
    Çıkış Yap
</button>

</div>

</div>

<div class="stats">

<div class="lives">
    ❤️ <span id="lives">3</span>/6
</div>

<div class="score">
    ⭐ <span id="score">30</span>
</div>

</div>

<div id="menu">

<h1>Türkçe Avcısı</h1>

<div class="subtitle">
7. Sınıf Fiiller & Yazım Kuralları
</div>

<button class="btn easy selected"
onclick="difficulty(30,this)">
🟢 Kolay - 30 Saniye
</button>

<button class="btn normal"
onclick="difficulty(15,this)">
🟡 Normal - 15 Saniye
</button>

<button class="btn hard"
onclick="difficulty(7,this)">
🔴 Zor - 7 Saniye
</button>

<button class="btn fast"
onclick="difficulty(4,this)">
⚡ FAST - 4 Saniye
</button>

<button class="btn start" onclick="startGame()">
OYUNA BAŞLA
</button>

<button class="btn shop" onclick="openShop()">
🛒 CAN DÜKKANI
</button>

</div>

<div id="game" class="hide">

<div class="hud">

<span>
Soru <span id="qNo">1</span>/10
</span>

<span>
⏱️ <span id="time">30</span>
</span>

</div>

<div id="category" class="category">
Kategori
</div>

<div id="question" class="question">
Soru
</div>

<div id="options" class="options"></div>

</div>

<div id="shop" class="hide">

<h1>CAN DÜKKANI</h1>

<div class="subtitle">
Puanlarınla can satın al
</div>

<div class="shop-item">

<span>
❤️ +1 CAN
<br>
<small>30 Puan</small>
</span>

<button class="buy" onclick="buy(1,30)">
Al
</button>

</div>

<div class="shop-item">

<span>
❤️❤️ +2 CAN
<br>
<small>60 Puan</small>
</span>

<button class="buy" onclick="buy(2,60)">
Al
</button>

</div>

<div class="shop-item">

<span>
❤️❤️❤️ +3 CAN
<br>
<small>90 Puan</small>
</span>

<button class="buy" onclick="buy(3,90)">
Al
</button>

</div>

<button class="btn start" onclick="menu()">
ANA MENÜ
</button>

</div>

<div id="end" class="hide">

<h1 id="endTitle">
TEBRİKLER!
</h1>

<div class="subtitle" id="endText">
Oyun tamamlandı.
</div>

<button class="btn start" onclick="menu()">
ANA MENÜ
</button>

<button class="btn shop" onclick="openShop()">
DÜKKAN
</button>

</div>

</div>

<script>

const questions=[

{
q:"Aşağıdaki cümlelerin hangisinde iş (kılış) fiili vardır?",
o:[
"Dün gece erkenden uyumuşum.",
"Balkondaki çiçekler sararmış.",
"Kardeşim kitabı okudu.",
"Yapraklar dökülür."
],
a:2,
c:"Fiilde Anlam"
},

{
q:"'Sararmak' fiili anlam özelliğine göre hangisidir?",
o:[
"İş fiili",
"Durum fiili",
"Oluş fiili",
"Tezlik fiili"
],
a:2,
c:"Fiilde Anlam"
},

{
q:"'Geleceksiniz' fiilinin kipi ve şahsı hangisidir?",
o:[
"Gelecek Zaman - 2. Çoğul",
"Şimdiki Zaman - 2. Çoğul",
"Gelecek Zaman - 3. Çoğul",
"Gereklilik - 2. Çoğul"
],
a:0,
c:"Fiil Çekimi"
},

{
q:"Aşağıdakilerden hangisi dilek kipidir?",
o:[
"Görülen Geçmiş Zaman",
"Geniş Zaman",
"Şart Kipi",
"Gelecek Zaman"
],
a:2,
c:"Fiil Kipleri"
},

{
q:"'Okumalısın' fiilinin taşıdığı anlam nedir?",
o:[
"İstek",
"Şart",
"Gereklilik",
"Emir"
],
a:2,
c:"Fiil Kipleri"
},

{
q:"'ki'nin yazımı hangisinde yanlıştır?",
o:[
"Duydum ki unutmuşsun.",
"Evdeki hesap çarşıya uymadı.",
"Senki her zaman yanımdaydın.",
"Mademki gelmeyecektin."
],
a:2,
c:"Yazım Kuralları"
},

{
q:"'de/da' bağlacının yazımı hangisinde doğrudur?",
o:[
"Sende bizimle gelsene.",
"Kitabıda evde unutmuşum.",
"Oğluda bize geldi.",
"Sınava o da katılacakmış."
],
a:3,
c:"Yazım Kuralları"
},

{
q:"Aşağıdaki sözcüklerden hangisinin yazımı doğrudur?",
o:[
"Rastgele",
"Şeyherşey",
"Yanlızca",
"Bir çok"
],
a:0,
c:"Yazım Kuralları"
},

{
q:"Hangisinde yazım yanlışı vardır?",
o:[
"Türk Dil Kurumu",
"29 Ekim 1923'te",
"Severek yapıyorum.",
"Hergün düzenli koşar."
],
a:3,
c:"Yazım Kuralları"
},

{
q:"Aşağıdakilerden hangisinin yazımı yanlıştır?",
o:[
"Traş",
"Kılavuz",
"Spor",
"Sürpriz"
],
a:0,
c:"Yazım Kuralları"
}

];

const MAX_LIVES=6;

let lives=3;
let score=30;
let gmail=null;
let questionIndex=0;
let gameQuestions=[];
let timeLimit=30;
let timer=null;
let timeLeft=0;
let answering=true;
let combo=0;


/* GMAIL */

function saveKey(){

    if(!gmail) return null;

    return "turkce_avcisi_"+gmail;
}

function save(){

    if(!gmail) return;

    const data={
        lives:lives,
        score:score
    };

    localStorage.setItem(
        saveKey(),
        JSON.stringify(data)
    );
}

function load(){

    if(!gmail) return;

    const data=
        localStorage.getItem(saveKey());

    if(!data){

        lives=3;
        score=30;

        save();

        return;
    }

    try{

        const obj=JSON.parse(data);

        lives=Math.max(
            0,
            Math.min(
                MAX_LIVES,
                Number(obj.lives)||3
            )
        );

        score=Math.max(
            0,
            Number(obj.score)||30
        );

    }catch(e){

        lives=3;
        score=30;

        save();
    }
}

function login(){

    const input=
        document.getElementById("gmail");

    const email=
        input.value.trim().toLowerCase();

    if(!email){

        alert(
            "Gmail adresini yazmalısın."
        );

        return;
    }

    if(!/^[^\s@]+@gmail\.com$/.test(email)){

        alert(
            "Lütfen geçerli bir @gmail.com adresi gir."
        );

        return;
    }

    gmail=email;

    localStorage.setItem(
        "turkce_avcisi_active",
        gmail
    );

    load();

    document
        .getElementById("loginArea")
        .classList.add("hide");

    document
        .getElementById("accountArea")
        .classList.remove("hide");

    document
        .getElementById("accountEmail")
        .textContent=gmail;

    update();

    alert(
        "Giriş başarılı!\n\n"+
        "İlerlemen bu tarayıcıda kaydedilecek."
    );
}

function logout(){

    gmail=null;

    localStorage.removeItem(
        "turkce_avcisi_active"
    );

    lives=3;
    score=30;
    combo=0;

    document
        .getElementById("loginArea")
        .classList.remove("hide");

    document
        .getElementById("accountArea")
        .classList.add("hide");

    document
        .getElementById("gmail")
        .value="";

    update();

    menu();
}

function restore(){

    const active=
        localStorage.getItem(
            "turkce_avcisi_active"
        );

    if(!active){

        update();

        return;
    }

    if(!/^[^\s@]+@gmail\.com$/.test(active)){
        return;
    }

    gmail=active;

    load();

    document
        .getElementById("loginArea")
        .classList.add("hide");

    document
        .getElementById("accountArea")
        .classList.remove("hide");

    document
        .getElementById("accountEmail")
        .textContent=gmail;

    update();
}


/* ARAYÜZ */

function update(){

    document.getElementById("lives")
        .textContent=lives;

    document.getElementById("score")
        .textContent=score;
}


/* ZORLUK */

function difficulty(seconds,button){

    timeLimit=seconds;

    document
        .querySelectorAll(
            ".easy,.normal,.hard,.fast"
        )
        .forEach(x=>{
            x.classList.remove("selected");
        });

    button.classList.add("selected");
}


/* OYUN */

function startGame(){

    if(lives<=0){

        alert(
            "Canın kalmadı. Dükkandan can alabilirsin."
        );

        return;
    }

    gameQuestions=
        [...questions]
        .sort(()=>Math.random()-.5);

    questionIndex=0;
    combo=0;

    show("game");
    hide("menu");
    hide("shop");
    hide("end");

    loadQuestion();
}

function loadQuestion(){

    clearInterval(timer);

    answering=true;

    const q=
        gameQuestions[questionIndex];

    document.getElementById("qNo")
        .textContent=
        questionIndex+1;

    document.getElementById("category")
        .textContent=q.c;

    document.getElementById("question")
        .textContent=q.q;

    const area=
        document.getElementById("options");

    area.innerHTML="";

    q.o.forEach((text,index)=>{

        const button=
            document.createElement("button");

        button.className="option";

        button.textContent=text;

        button.onclick=()=>{
            answer(index,button);
        };

        area.appendChild(button);
    });

    timeLeft=timeLimit;

    document.getElementById("time")
        .textContent=timeLeft;

    timer=setInterval(()=>{

        timeLeft--;

        document.getElementById("time")
            .textContent=timeLeft;

        if(timeLeft<=0){

            clearInterval(timer);

            timeout();
        }

    },1000);
}


/* COMBO */

function showCombo(){

    if(combo<2) return;

    const element=
        document.getElementById("combo");

    element.textContent=
        "🔥 COMBO "+combo+"! +10 PUAN 🔥";

    element.classList.remove("show");

    void element.offsetWidth;

    element.classList.add("show");
}


/* CEVAP */

function answer(index,button){

    if(!answering) return;

    answering=false;

    clearInterval(timer);

    const q=
        gameQuestions[questionIndex];

    const buttons=
        document.querySelectorAll(".option");

    if(index===q.a){

        button.classList.add("correct");

        combo++;

        score+=10+timeLeft;

        showCombo();

        score+=10;

    }else{

        button.classList.add("wrong");

        if(buttons[q.a]){
            buttons[q.a]
                .classList.add("correct");
        }

        lives--;

        combo=0;
    }

    update();

    save();

    setTimeout(()=>{

        if(lives<=0){

            finish(true);

            return;
        }

        questionIndex++;

        if(
            questionIndex>=gameQuestions.length
        ){

            finish(false);

        }else{

            loadQuestion();
        }

    },1000);
}


function timeout(){

    if(!answering) return;

    answering=false;

    const q=
        gameQuestions[questionIndex];

    const buttons=
        document.querySelectorAll(".option");

    if(buttons[q.a]){
        buttons[q.a]
            .classList.add("correct");
    }

    lives--;
    combo=0;

    update();

    save();

    setTimeout(()=>{

        if(lives<=0){

            finish(true);

            return;
        }

        questionIndex++;

        if(
            questionIndex>=gameQuestions.length
        ){

            finish(false);

        }else{

            loadQuestion();
        }

    },1000);
}


/* BİTİŞ */

function finish(noLives){

    clearInterval(timer);

    save();

    hide("game");
    show("end");

    if(noLives){

        document
            .getElementById("endTitle")
            .textContent=
            "CANLARIN BİTTİ!";

        document
            .getElementById("endText")
            .textContent=
            "Dükkandan can alarak devam edebilirsin.";

    }else{

        document
            .getElementById("endTitle")
            .textContent=
            "TEBRİKLER!";

        document
            .getElementById("endText")
            .textContent=
            "10 soruyu tamamladın!";
    }
}


/* DÜKKAN */

function buy(amount,cost){

    if(lives+amount>MAX_LIVES){

        alert(
            "En fazla 6 can olabilir."
        );

        return;
    }

    if(score<cost){

        alert(
            "Yeterli puanın yok."
        );

        return;
    }

    score-=cost;
    lives+=amount;

    update();

    save();
}

function openShop(){

    hide("menu");
    hide("game");
    hide("end");

    show("shop");
}

function menu(){

    clearInterval(timer);

    hide("game");
    hide("shop");
    hide("end");

    show("menu");

    update();
}


/* YARDIMCI */

function show(id){

    document
        .getElementById(id)
        .classList
        .remove("hide");
}

function hide(id){

    document
        .getElementById(id)
        .classList
        .add("hide");
}


/* ENTER */

document
    .getElementById("gmail")
    .addEventListener(
        "keydown",
        function(e){

            if(e.key==="Enter"){
                login();
            }

        }
    );


/* ARKA PLAN */

const canvas=
    document.getElementById("bg");

const ctx=
    canvas.getContext("2d");

let particles=[];

function resize(){

    canvas.width=
        window.innerWidth;

    canvas.height=
        window.innerHeight;
}

function createParticles(){

    particles=[];

    for(let i=0;i<100;i++){

        particles.push({

            x:Math.random()*canvas.width,

            y:Math.random()*canvas.height,

            r:Math.random()*3+1,

            speed:Math.random()*1.5+.3

        });
    }
}

function animate(){

    ctx.clearRect(
        0,
        0,
        canvas.width,
        canvas.height
    );

    ctx.fillStyle="#050508";

    ctx.fillRect(
        0,
        0,
        canvas.width,
        canvas.height
    );

    particles.forEach(p=>{

        p.y+=p.speed;

        if(p.y>canvas.height){

            p.y=-5;

            p.x=
                Math.random()*canvas.width;
        }

        ctx.fillStyle=
            "rgba(0,242,254,.35)";

        ctx.beginPath();

        ctx.arc(
            p.x,
            p.y,
            p.r,
            0,
            Math.PI*2
        );

        ctx.fill();
    });

    requestAnimationFrame(animate);
}

window.addEventListener(
    "resize",
    ()=>{
        resize();
        createParticles();
    }
);

resize();
createParticles();
animate();

restore();
update();

</script>

</body>
</html>
