Five Nights at Freddy's (FNAF) mekaniklerini en sade ama oynanabilir haliyle bir araya getirdim. Bu sürümde; enerji yönetimi, kameralar, ışıklar, kapılar ve sana doğru yaklaşan bir animatronic (Bonnie) bulunuyor.

Bu kodu bir .html dosyası olarak kaydet ve tarayıcında aç.

Basit FNAF: Gece Nöbeti
HTML
<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <title>Mini FNAF Projesi</title>
    <style>
        body { margin: 0; background: #000; color: #fff; font-family: 'Courier New', monospace; overflow: hidden; user-select: none; }
        
        /* Ana Ofis Alanı */
        #office {
            width: 100vw; height: 100vh;
            display: flex; justify-content: center; align-items: center;
            background: radial-gradient(circle, #333 0%, #000 70%);
            transition: background 0.2s;
        }

        /* Kapı ve Işık Durumları */
        .door-overlay {
            position: absolute; width: 15%; height: 80%;
            background: #111; border: 4px solid #444; top: 10%;
            display: none; z-index: 5;
        }
        #left-door { left: 5%; }
        #right-door { right: 5%; }

        /* Kamera Ekranı */
        #cam-screen {
            display: none; position: fixed; top: 0; left: 0;
            width: 100%; height: 100%; background: #000; z-index: 20;
            border: 15px solid #222; box-sizing: border-box;
        }
        .static {
            position: absolute; width: 100%; height: 100%;
            background: url('https://media.giphy.com/media/oEI9uWUicG5U4/giphy.gif');
            opacity: 0.15; pointer-events: none;
        }

        /* UI Paneli */
        #ui {
            position: fixed; bottom: 20px; width: 100%;
            display: flex; justify-content: space-around; z-index: 30;
        }
        button {
            padding: 15px 25px; background: #222; color: #eee;
            border: 2px solid #555; cursor: pointer; font-weight: bold;
        }
        button:active { background: #444; }
        .stats { position: fixed; top: 20px; left: 20px; font-size: 24px; z-index: 30; }

        /* JumpScare */
        #jumpscare {
            display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: red; z-index: 100; justify-content: center; align-items: center;
        }
    </style>
</head>
<body>

<div class="stats">
    GÜÇ: %<span id="power">100</span><br>
    SAAT: <span id="time">12</span> AM
</div>

<div id="office">
    <div id="left-door" class="door-overlay"></div>
    <div id="right-door" class="door-overlay"></div>
    <h2 id="office-msg">OFİS - ÖNÜNE BAKILIYOR</h2>
</div>

<div id="cam-screen">
    <div class="static"></div>
    <div style="padding: 50px;">
        <h3>GÜVENLİK KAMERASI 01A</h3>
        <p id="cam-status">KORİDORDA KİMSE YOK.</p>
    </div>
</div>

<div id="jumpscare"><h1>BOOO! YAKALANDIN!</h1></div>

<div id="ui">
    <button onclick="toggleDoor('left')">SOL KAPI</button>
    <button onclick="toggleLight()">IŞIK</button>
    <button onclick="toggleCam()">KAMERA</button>
    <button onclick="toggleDoor('right')">SAĞ KAPI</button>
</div>

<script>
    let power = 100;
    let hour = 12;
    let leftDoorOpen = true;
    let rightDoorOpen = true;
    let camOpen = false;
    let lightOpen = false;
    let enemyPos = 0; // 0: Sahne, 1: Koridor, 2: Kapı Eşiği, 3: Saldırı!

    // Enerji ve Zaman Döngüsü
    const gameTick = setInterval(() => {
        // Enerji Azaltma
        let consumption = 0.5;
        if (!leftDoorOpen) consumption += 1.5;
        if (!rightDoorOpen) consumption += 1.5;
        if (camOpen) consumption += 1.0;
        if (lightOpen) consumption += 0.5;
        
        power -= consumption;
        document.getElementById('power').innerText = Math.floor(power);

        if (power <= 0) gameOver("Enerji Bitti!");

        // Düşman Hareketi (Her 5 saniyede bir şans)
        if (Math.random() > 0.7) {
            enemyPos++;
            updateCamStatus();
        }

        // Saldırı Kontrolü
        if (enemyPos === 3) {
            if (!leftDoorOpen) {
                enemyPos = 0; // Kapı kapalıysa geri gider
                updateCamStatus();
            } else if (!camOpen) {
                gameOver("Bonnie seni yakaladı!");
            }
        }
    }, 3000);

    // Saat Döngüsü
    setInterval(() => {
        hour++;
        if (hour > 12) hour = 1;
        document.getElementById('time').innerText = hour;
        if (hour === 6) {
            alert("Kazandın! Saat 06:00");
            location.reload();
        }
    }, 45000); // Her 45 saniyede bir saat geçer

    function toggleDoor(side) {
        if (side === 'left') {
            leftDoorOpen = !leftDoorOpen;
            document.getElementById('left-door').style.display = leftDoorOpen ? 'none' : 'block';
        } else {
            rightDoorOpen = !rightDoorOpen;
            document.getElementById('right-door').style.display = rightDoorOpen ? 'none' : 'block';
        }
    }

    function toggleCam() {
        camOpen = !camOpen;
        document.getElementById('cam-screen').style.display = camOpen ? 'block' : 'none';
        updateCamStatus();
    }

    function toggleLight() {
        lightOpen = !lightOpen;
        const office = document.getElementById('office');
        if (lightOpen) {
            office.style.background = (enemyPos === 2) ? "radial-gradient(circle, #f00 0%, #333 70%)" : "radial-gradient(circle, #666 0%, #222 70%)";
            document.getElementById('office-msg').innerText = (enemyPos === 2) ? "BİRİ VAR!" : "KİMSE YOK";
        } else {
            office.style.background = "radial-gradient(circle, #333 0%, #000 70%)";
            document.getElementById('office-msg').innerText = "OFİS - ÖNÜNE BAKILIYOR";
        }
    }

    function updateCamStatus() {
        const status = document.getElementById('cam-status');
        if (enemyPos === 0) status.innerText = "Bonnie sahnede bekliyor.";
        else if (enemyPos === 1) status.innerText = "Karanlık koridorda bir karaltı var...";
        else if (enemyPos === 2) status.innerText = "KAMERA ARIZASI! (Kapıya çok yakın!)";
    }

    function gameOver(reason) {
        clearInterval(gameTick);
        document.getElementById('jumpscare').style.display = 'flex';
        setTimeout(() => {
            alert("OYUN BİTTİ: " + reason);
            location.reload();
        }, 1000);
    }
</script>

</body>
</html>
