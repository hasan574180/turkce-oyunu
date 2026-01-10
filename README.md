<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>7. Sınıf Fiiller - Ultra Maraton</title>
    <style>
        :root { --primary: #1a1a1a; --secondary: #2d3436; --accent: #0984e3; --success: #00b894; --danger: #d63031; --ultra: #6c5ce7; }
        body { margin: 0; font-family: 'Segoe UI', sans-serif; background: var(--primary); color: white; display: flex; align-items: center; justify-content: center; height: 100vh; overflow: hidden; }
        #game-container { width: 92%; max-width: 500px; background: var(--secondary); padding: 25px; border-radius: 20px; box-shadow: 0 15px 35px rgba(0,0,0,0.8); text-align: center; border: 1px solid #444; position: relative; }
        .hidden { display: none; }
        .btn { border: none; padding: 15px; color: white; border-radius: 10px; font-size: 18px; cursor: pointer; margin: 8px 0; width: 100%; transition: 0.3s; font-weight: bold; }
        .btn-ultra { background: var(--ultra); animation: lightning 1s infinite; }
        @keyframes lightning { 0% { box-shadow: 0 0 5px var(--ultra); } 50% { box-shadow: 0 0 20px var(--ultra); } 100% { box-shadow: 0 0 5px var(--ultra); } }
        .btn-next { background: #fdcb6e; color: #2d3436; margin-top: 20px; font-size: 20px; }
        #timer { font-size: 35px; font-weight: bold; color: #ffeaa7; margin-bottom: 10px; }
        #question { font-size: 20px; margin-bottom: 20px; min-height: 70px; color: #dfe6e9; }
        .option { background: rgba(255,255,255,0.05); border: 2px solid rgba(255,255,255,0.1); padding: 12px; margin: 8px 0; border-radius: 10px; cursor: pointer; text-align: left; transition: 0.2s; font-size: 17px; }
        .correct { background: var(--success) !important; border-color: #fff !important; }
        .wrong { background: var(--danger) !important; border-color: #fff !important; }
        #feedback { color: #fab1a0; font-weight: bold; margin-top: 10px; font-size: 16px; border: 1px dashed #fab1a0; padding: 5px; border-radius: 5px; }
        
        /* İmza Kısmı */
        .developer-credit { 
            margin-top: 20px; 
            font-size: 13px; 
            color: #888; 
            font-style: italic;
            border-top: 1px solid rgba(255,255,255,0.1);
            padding-top: 10px;
        }
        .dev-name {
            color: #fdcb6e;
            font-weight: bold;
            text-shadow: 0 0 5px rgba(253, 203, 110, 0.3);
        }
    </style>
</head>
<body>

<div id="game-container">
    <div id="start-screen">
        <h1 style="color: var(--ultra); text-shadow: 0 0 10px rgba(108, 92, 231, 0.5); margin-bottom: 5px;">Fiil Şimşeği 7. Sınıf</h1>
        <p style="margin-top: 0; margin-bottom: 20px; color: #aaa;">Geliştirici: <span class="dev-name">Hasan Hüseyin Mert</span></p>
        <p>Hızını ve Bilgini Seç!</p>
        <button class="btn" style="background:var(--success)" onclick="startGame(30)">KOLAY (30sn)</button>
        <button class="btn" style="background:#e17055" onclick="startGame(20)">ORTA (20sn)</button>
        <button class="btn" style="background:var(--danger)" onclick="startGame(7)">ZOR (7sn)</button>
        <button class="btn btn-ultra" onclick="startGame(4)">ULTRA ZOR (4 Saniye!)</button>
    </div>

    <div id="quiz-screen" class="hidden">
        <div id="timer">4</div>
        <div id="progress" style="color: #636e72; font-size: 14px;">Soru: 1 / 15</div>
        <div id="question">Soru yükleniyor...</div>
        <div id="options"></div>
        <div id="feedback" class="hidden">HATA! Doğruyu incele (Analiz Süresi: 30sn)</div>
        <button id="next-btn" class="btn btn-next hidden" onclick="nextQuestion()">SIRADAKİ SORU ➔</button>
    </div>

    <div id="result-screen" class="hidden">
        <h1>Görev Tamamlandı!</h1>
        <p id="final-score" style="font-size: 26px; color: var(--success);"></p>
        <button class="btn" style="background: var(--accent);" onclick="location.reload()">Tekrar Dene</button>
    </div>

    <div class="developer-credit">
        Hasan Hüseyin Mert Tarafından Geliştirildi
    </div>
</div>

<script>
    const questions = [
        { q: "'Baktı' fiili anlamca hangisidir?", a: ["İş", "Oluş", "Durum", "Yapı"], c: 2 },
        { q: "'Onu' kelimesini alan fiil türü?", a: ["Durum", "Oluş", "Kılış (İş)", "Basit"], c: 2 },
        { q: "Hangi fiil kendiliğinden olur?", a: ["Atmak", "Solmak", "Gülmek", "Yazmak"], c: 1 },
        { q: "'Gideceğiz' kişisi hangisidir?", a: ["Biz", "Siz", "Onlar", "Ben"], c: 0 },
        { q: "Hangisi bildirme (haber) kipidir?", a: ["İstek", "Şart", "Gereklilik", "Şimdiki Zaman"], c: 3 },
        { q: "'Git-meli' kipi hangisidir?", a: ["Emir", "Şart", "Gereklilik", "İstek"], c: 2 },
        { q: "'Bak-a-kal-dı' türü nedir?", a: ["Tezlik", "Sürerlik", "Yaklaşma", "Yeterlilik"], c: 1 },
        { q: "Hangisi türemiş fiildir?", a: ["Geldi", "Sevindi", "Okudu", "Baktı"], c: 1 },
        { q: "Hangisinde anlam kayması var?", a: ["Dün gitti", "Her gün okuyor", "Yarın gelecek", "Bakıyor"], c: 1 },
        { q: "Ek fiil ismi ne yapar?", a: ["Sıfat", "Yüklem", "Zarf", "Edat"], c: 1 },
        { q: "'Gelecekmiş' hangi birleşik zaman?", a: ["Hikaye", "Rivayet", "Şart", "Basit"], c: 1 },
        { q: "'Kayboldu' yapısı nedir?", a: ["Basit", "Türemiş", "Birleşik", "Yansıma"], c: 2 },
        { q: "'Okut-' fiilinin kökü nedir?", a: ["Oku (Fiil)", "Ok (İsim)", "Okut (Fiil)", "Okul (İsim)"], c: 0 },
        { q: "'Koşmalısın' kişisi?", a: ["Ben", "Sen", "O", "Siz"], c: 1 },
        { q: "'Yap-a-bil-ir' hangi anlam?", a: ["Tezlik", "Sürerlik", "Yeterlilik", "Yaklaşma"], c: 2 }
    ];

    let currentQ = 0, score = 0, timeLeft = 0, timerInterval, baseTime = 0, canClick = true;

    function startGame(time) {
        baseTime = time;
        document.getElementById('start-screen').classList.add('hidden');
        document.getElementById('quiz-screen').classList.remove('hidden');
        showQuestion();
    }

    function showQuestion() {
        if (currentQ >= 15) return endGame();
        canClick = true;
        timeLeft = baseTime;
        
        document.getElementById('timer').innerText = timeLeft;
        document.getElementById('progress').innerText = `Soru: ${currentQ + 1} / 15`;
        document.getElementById('question').innerText = questions[currentQ].q;
        document.getElementById('feedback').classList.add('hidden');
        document.getElementById('next-btn').classList.add('hidden');
        
        const optionsDiv = document.getElementById('options');
        optionsDiv.innerHTML = '';
        
        questions[currentQ].a.forEach((opt, index) => {
            const div = document.createElement('div');
            div.className = 'option';
            div.id = 'opt-' + index;
            div.innerText = opt;
            div.onclick = () => checkAnswer(index);
            optionsDiv.appendChild(div);
        });

        startTimer(baseTime);
    }

    function startTimer(duration) {
        clearInterval(timerInterval);
        timeLeft = duration;
        document.getElementById('timer').innerText = timeLeft;
        timerInterval = setInterval(() => {
            timeLeft--;
            if (timeLeft < 0) {
                clearInterval(timerInterval);
                if(canClick) checkAnswer(-1); 
            } else {
                document.getElementById('timer').innerText = timeLeft;
            }
        }, 1000);
    }

    function checkAnswer(index) {
        if (!canClick) return;
        canClick = false;
        clearInterval(timerInterval);

        const correctIdx = questions[currentQ].c;
        const correctDiv = document.getElementById('opt-' + correctIdx);

        if (index === correctIdx) {
            document.getElementById('opt-' + index).classList.add('correct');
            score++;
            setTimeout(() => { nextQuestion(); }, 700);
        } else {
            if(index !== -1) document.getElementById('opt-' + index).classList.add('wrong');
            correctDiv.classList.add('correct');
            
            document.getElementById('feedback').classList.remove('hidden');
            document.getElementById('next-btn').classList.remove('hidden');
            
            startTimer(30); 
        }
    }

    function nextQuestion() {
        currentQ++;
        showQuestion();
    }

    function endGame() {
        document.getElementById('quiz-screen').classList.add('hidden');
        document.getElementById('result-screen').classList.remove('hidden');
        document.getElementById('final-score').innerText = `15 Soruda ${score} Doğru!`;
    }
</script>

</body>
</html>

