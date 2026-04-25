Harika bir fikir! Web tarayıcısı üzerinde çalışan, hem fareyle tıklanabilen hem de klavyedeki tuşlarla kontrol edilebilen bir piyano yapmak oldukça eğlencelidir.

Bu projede sesleri üretmek için harici ses dosyalarına ihtiyaç duymadan, tarayıcının kendi Web Audio API özelliğini kullanacağız. Böylece kod tek başına (standalone) çalışabilecek.

HTML/JS Piyano Kodu
Bu kodu bir .html dosyası olarak kaydedip tarayıcında açabilirsin.

HTML
<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <title>JS Dijital Piyano</title>
    <style>
        body {
            background: #222;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
            color: white;
            font-family: sans-serif;
        }

        .piano {
            display: flex;
            position: relative;
        }

        .key {
            width: 60px;
            height: 200px;
            border: 1px solid #000;
            background: white;
            border-radius: 0 0 5px 5px;
            cursor: pointer;
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
            align-items: center;
            padding-bottom: 10px;
            color: #333;
            font-weight: bold;
            transition: background 0.1s;
            user-select: none;
        }

        .key.active {
            background: #ddd;
            transform: translateY(2px);
        }

        /* Siyah Tuşlar */
        .black-key {
            width: 40px;
            height: 120px;
            background: #000;
            color: white;
            position: absolute;
            z-index: 2;
            margin-left: -20px;
            border-radius: 0 0 3px 3px;
        }

        .black-key.active {
            background: #444;
        }

        .controls { margin-bottom: 20px; text-align: center; }
    </style>
</head>
<body>

<div class="controls">
    <h1>JS Klavye Piyano</h1>
    <p>Tuşlara tıkla veya klavyendeki harfleri kullan: <strong>A, S, D, F, G, H, J, K</strong></p>
</div>

<div class="piano" id="piano">
    <div class="key" data-note="C4" data-key="A">A <span>(Do)</span></div>
    <div class="key" data-note="D4" data-key="S">S <span>(Re)</span></div>
    <div class="key" data-note="E4" data-key="D">D <span>(Mi)</span></div>
    <div class="key" data-note="F4" data-key="F">F <span>(Fa)</span></div>
    <div class="key" data-note="G4" data-key="G">G <span>(Sol)</span></div>
    <div class="key" data-note="A4" data-key="H">H <span>(La)</span></div>
    <div class="key" data-note="B4" data-key="J">J <span>(Si)</span></div>
    <div class="key" data-note="C5" data-key="K">K <span>(Do)</span></div>
</div>

<script>
    // Ses dalgaları için frekans tablosu
    const notes = {
        "A": 261.63, // C4 (Do)
        "S": 293.66, // D4 (Re)
        "D": 329.63, // E4 (Mi)
        "F": 349.23, // F4 (Fa)
        "G": 392.00, // G4 (Sol)
        "H": 440.00, // A4 (La)
        "J": 493.88, // B4 (Si)
        "K": 523.25  // C5 (Do)
    };

    const audioCtx = new (window.AudioContext || window.webkitAudioContext)();

    function playNote(key) {
        if (!notes[key]) return;

        // Ses oluşturma (Oscillator)
        const oscillator = audioCtx.createOscillator();
        const gainNode = audioCtx.createGain();

        oscillator.type = 'triangle'; // Piyano benzeri yumuşak bir ses
        oscillator.frequency.setValueAtTime(notes[key], audioCtx.currentTime);

        // Sesin yavaşça sönmesi (Fade out)
        gainNode.gain.setValueAtTime(0.5, audioCtx.currentTime);
        gainNode.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 1);

        oscillator.connect(gainNode);
        gainNode.connect(audioCtx.destination);

        oscillator.start();
        oscillator.stop(audioCtx.currentTime + 1);

        // Görsel efekt
        const el = document.querySelector(`[data-key="${key}"]`);
        if(el) {
            el.classList.add('active');
            setTimeout(() => el.classList.remove('active'), 100);
        }
    }

    // Fare tıklama dinleyici
    document.querySelectorAll('.key').forEach(keyEl => {
        keyEl.addEventListener('mousedown', () => {
            playNote(keyEl.getAttribute('data-key'));
        });
    });

    // Klavye tuş dinleyici
    window.addEventListener('keydown', (e) => {
        const key = e.key.toUpperCase();
        if (notes[key]) {
            playNote(key);
        }
    });
</script>

</body>
</html>
