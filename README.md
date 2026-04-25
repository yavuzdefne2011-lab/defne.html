<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <title>Gelişmiş Web Piyano</title>
    <style>
        body {
            background: #1a1a1a;
            display: flex; flex-direction: column;
            align-items: center; justify-content: center;
            height: 100vh; margin: 0; color: white; font-family: 'Segoe UI', sans-serif;
        }

        .piano-container {
            position: relative;
            display: flex;
            padding: 20px;
            background: #333;
            border-radius: 10px;
            box-shadow: 0 20px 50px rgba(0,0,0,0.5);
        }

        .key {
            position: relative;
            cursor: pointer;
            user-select: none;
        }

        /* Beyaz Tuşlar */
        .white {
            width: 60px; height: 230px;
            background: white;
            border: 1px solid #ccc;
            border-radius: 0 0 5px 5px;
            display: flex; flex-direction: column;
            justify-content: flex-end; align-items: center;
            padding-bottom: 15px; color: #333; font-weight: bold;
            z-index: 1; transition: background 0.1s, transform 0.05s;
        }

        .white.active {
            background: #e0e0e0;
            transform: translateY(2px);
            box-shadow: inset 0 5px 10px rgba(0,0,0,0.2);
        }

        /* Siyah Tuşlar */
        .black {
            width: 40px; height: 140px;
            background: #000;
            color: white;
            position: absolute;
            z-index: 2;
            margin-left: -20px; /* Bir önceki beyaz tuşun üzerine binmesi için */
            border-radius: 0 0 3px 3px;
            display: flex; flex-direction: column;
            justify-content: flex-end; align-items: center;
            padding-bottom: 10px; font-size: 12px;
            transition: background 0.1s, transform 0.05s;
        }

        .black.active {
            background: #444;
            transform: translateY(2px);
        }

        .label { pointer-events: none; }
        .hint { font-size: 10px; opacity: 0.6; margin-top: 5px; }

        .controls { margin-bottom: 30px; text-align: center; }
        kbd {
            background: #444; padding: 2px 6px; border-radius: 4px;
            border: 1px solid #666; font-family: monospace;
        }
    </style>
</head>
<body>

<div class="controls">
    <h1>Profesyonel Klavye Piyano</h1>
    <p>Beyazlar: <kbd>A</kbd> <kbd>S</kbd> <kbd>D</kbd> <kbd>F</kbd> <kbd>G</kbd> <kbd>H</kbd> <kbd>J</kbd> <kbd>K</kbd> <kbd>L</kbd> <kbd>;</kbd></p>
    <p>Siyahlar: <kbd>W</kbd> <kbd>E</kbd> <kbd>T</kbd> <kbd>Y</kbd> <kbd>U</kbd> <kbd>O</kbd> <kbd>P</kbd></p>
</div>

<div class="piano-container" id="piano">
    <div class="key white" data-note="C4" data-key="A"><span class="label">A</span><span class="hint">Do</span></div>
    <div class="key black" data-note="C#4" data-key="W" style="left: 80px;"><span class="label">W</span></div>
    
    <div class="key white" data-note="D4" data-key="S"><span class="label">S</span><span class="hint">Re</span></div>
    <div class="key black" data-note="D#4" data-key="E" style="left: 140px;"><span class="label">E</span></div>
    
    <div class="key white" data-note="E4" data-key="D"><span class="label">D</span><span class="hint">Mi</span></div>
    
    <div class="key white" data-note="F4" data-key="F"><span class="label">F</span><span class="hint">Fa</span></div>
    <div class="key black" data-note="F#4" data-key="T" style="left: 260px;"><span class="label">T</span></div>
    
    <div class="key white" data-note="G4" data-key="G"><span class="label">G</span><span class="hint">Sol</span></div>
    <div class="key black" data-note="G#4" data-key="Y" style="left: 320px;"><span class="label">Y</span></div>
    
    <div class="key white" data-note="A4" data-key="H"><span class="label">H</span><span class="hint">La</span></div>
    <div class="key black" data-note="A#4" data-key="U" style="left: 380px;"><span class="label">U</span></div>
    
    <div class="key white" data-note="B4" data-key="J"><span class="label">J</span><span class="hint">Si</span></div>
    
    <div class="key white" data-note="C5" data-key="K"><span class="label">K</span><span class="hint">Do</span></div>
    <div class="key black" data-note="C#5" data-key="O" style="left: 500px;"><span class="label">O</span></div>
    
    <div class="key white" data-note="D5" data-key="L"><span class="label">L</span><span class="hint">Re</span></div>
    <div class="key black" data-note="D#5" data-key="P" style="left: 560px;"><span class="label">P</span></div>
    
    <div class="key white" data-note="E5" data-key=";"><span class="label">;</span><span class="hint">Mi</span></div>
</div>

<script>
    const audioCtx = new (window.AudioContext || window.webkitAudioContext)();

    // Nota Frekansları
    const noteFrequencies = {
        "A": 261.63, "W": 277.18, "S": 293.66, "E": 311.13, "D": 329.63,
        "F": 349.23, "T": 369.99, "G": 392.00, "Y": 415.30, "H": 440.00,
        "U": 466.16, "J": 493.88, "K": 523.25, "O": 554.37, "L": 587.33,
        "P": 622.25, ";": 659.25
    };

    const activeOscillators = {};

    function playNote(key) {
        if (!noteFrequencies[key] || activeOscillators[key]) return;

        const osc = audioCtx.createOscillator();
        const gain = audioCtx.createGain();

        osc.type = 'sine'; // Daha temiz, piyano benzeri tını
        osc.frequency.setValueAtTime(noteFrequencies[key], audioCtx.currentTime);

        gain.gain.setValueAtTime(0.3, audioCtx.currentTime);
        gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 1.5);

        osc.connect(gain);
        gain.connect(audioCtx.destination);

        osc.start();
        osc.stop(audioCtx.currentTime + 1.5);

        activeOscillators[key] = osc;

        const el = document.querySelector(`[data-key="${key}"]`);
        if(el) el.classList.add('active');
    }

    function stopNote(key) {
        if (activeOscillators[key]) {
            delete activeOscillators[key];
            const el = document.querySelector(`[data-key="${key}"]`);
            if(el) el.classList.remove('active');
        }
    }

    // Fare Etkinlikleri
    document.querySelectorAll('.key').forEach(keyEl => {
        keyEl.addEventListener('mousedown', () => playNote(keyEl.getAttribute('data-key')));
        keyEl.addEventListener('mouseup', () => stopNote(keyEl.getAttribute('data-key')));
        keyEl.addEventListener('mouseleave', () => stopNote(keyEl.getAttribute('data-key')));
    });

    // Klavye Etkinlikleri
    window.addEventListener('keydown', (e) => {
        const key = e.key === ';' ? ';' : e.key.toUpperCase();
        playNote(key);
    });

    window.addEventListener('keyup', (e) => {
        const key = e.key === ';' ? ';' : e.key.toUpperCase();
        stopNote(key);
    });
</script>

</body>
</html>
