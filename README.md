<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <title>Basit FNAF Prototipi</title>
    <style>
        body { margin: 0; background: #000; color: white; font-family: 'Courier New', Courier, monospace; overflow: hidden; }
        
        /* Ofis Görünümü */
        #office {
            width: 100vw; height: 100vh;
            background: linear-gradient(to bottom, #222, #000);
            display: flex; flex-direction: column;
            justify-content: center; align-items: center;
        }

        /* Kamera Ekranı */
        #camera-overlay {
            display: none;
            position: fixed; top: 0; left: 0;
            width: 100%; height: 100%;
            background: rgba(0, 255, 0, 0.1);
            border: 20px solid #333; box-sizing: border-box;
        }

        .static {
            background: url('https://media.giphy.com/media/oEI9uWUicG5U4/giphy.gif');
            opacity: 0.2; position: absolute; width: 100%; height: 100%;
        }

        /* Butonlar */
        #ui { position: fixed; bottom: 20px; width: 100%; text-align: center; }
        button {
            padding: 15px 30px; font-size: 20px;
            background: #444; color: white; border: 2px solid #888;
            cursor: pointer;
        }
        button:hover { background: #666; }

        /* Jump Scare */
        #jumpscare {
            display: none; position: fixed; top: 0; left: 0;
            width: 100%; height: 100%; background: red;
            color: black; font-size: 100px; font-weight: bold;
            text-align: center; line-height: 100vh; z-index: 100;
        }
    </style>
</head>
<body>

<div id="office">
    <h1>GÜVENLİK OFİSİ</h1>
    <p>Enerji: %<span id="power">100</span></p>
</div>

<div id="camera-overlay">
    <div class="static"></div>
    <h2 style="position:absolute; top:20px; left:20px;">KAMERA 01: DOĞU KORİDORU</h2>
</div>

<div id="jumpscare">ÖLDÜN!</div>

<div id="ui">
    <button onclick="toggleCamera()">KAMERAYI AÇ/KAPAT</button>
</div>

<script>
    let power = 100;
    let camOpen = false;

    // Enerji Azalma Döngüsü
    setInterval(() => {
        if(power > 0) {
            power -= camOpen ? 2 : 1; // Kamera açıkken enerji daha hızlı biter
            document.getElementById('power').innerText = power;
        } else {
            alert("Enerji bitti! Karanlıktasın...");
            location.reload();
        }
    }, 2000);

    function toggleCamera() {
        camOpen = !camOpen;
        const camView = document.getElementById('camera-overlay');
        camView.style.display = camOpen ? 'block' : 'none';
        
        // Rastgele Jump Scare şansı (%10)
        if(camOpen && Math.random() < 0.1) {
            triggerJumpscare();
        }
    }

    function triggerJumpscare() {
        document.getElementById('jumpscare').style.display = 'block';
        setTimeout(() => {
            location.reload();
        }, 2000);
    }
</script>

</body>
</html>
