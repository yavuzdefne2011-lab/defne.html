<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <title>2 Oktav Profesyonel Piyano</title>
    <style>
        body { 
            background: #121212;
            display: flex; flex-direction: column;
            align-items: center; justify-content: center;
            height: 100vh; margin: 0; color: white; font-family: 'Segoe UI', sans-serif;
        }
 
        .piano-wrapper {
            background: #2c2c2c;
            padding: 40px;
            border-radius: 15px;
            border-bottom: 10px solid #1a1a1a;
            box-shadow: 0 30px 60px rgba(0,0,0,0.8);
        }

        .piano-keys {
            display: flex;
            position: relative;
        }

        .key {
            position: relative;
            cursor: pointer;
            transition: all 0.05s;
            user-select: none;
        }

        /* Beyaz Tuşlar */
        .white {
            width: 50px; height: 250px;
            background: linear-gradient(to bottom, #eee 0%, #fff 100%);
            border: 1px solid #bbb;
            border-radius: 0 0 5px 5px;
            z-index: 1;
            display: flex; flex-direction: column;
            justify-content: flex-end; align-items: center;
            padding-bottom: 15px; color: #555; font-size: 14px; font-weight: bold;
        }

        .white.active {
            background: #ddd;
            transform: translateY(3px);
            box-shadow: inset 0 5px 15px rgba(0,0,0,0.3);
        }

        /* Siyah Tuşlar */
        .black {
            width: 34px; height: 150px;
            background: linear-gradient(to bottom, #444 0%, #000 100%);
            border: 1px solid #000;
            border-radius: 0 0 3px 3px;
            position: absolute;
            z-index: 2;
            margin-left: -17px;
            display: flex; flex-direction: column;
            justify-content: flex-end; align-items: center;
            padding-bottom: 10px; color: #fff; font-size: 11px;
        }

        .black.active {
            background: #333;
            transform: translateY(3px);
        }

        .info { margin-bottom: 20px; text-align: center; }
        kbd {
            background: #444; border: 1px solid #666;
            padding: 2px 5px; border-radius: 3px; font-family: monospace;
        }
    </style>
</head>
<body>

<div class="info">
    <h2>2 Oktav Dijital Piyano</h2>
    <p>Alt Oktav: <kbd>Z</kbd> <kbd>X</kbd> <kbd>C</kbd> <kbd>V</kbd> <kbd>B</kbd> <kbd>N</kbd> <kbd>M</kbd> | Üst Oktav: <kbd>Q</kbd> <kbd>W</kbd> <kbd>E</kbd> <kbd>R</kbd> <kbd>T</kbd> <kbd>Y</kbd> <kbd>U</kbd></p>
</div>

<div class="piano-wrapper">
    <div class="piano-keys" id="piano">
        <div class="key white" data-key="Z" data-freq="261.63">Z</div>
        <div class="key black" data-key="S" data-freq="277.18" style="left: 50px;">S</div>
        <div class="key white" data-key="X" data-freq="293.66">X</div>
        <div class="key black" data-key="D" data-freq="311.13" style="left: 100px;">D</div>
        <div class="key white" data-key="C" data-freq="329.63">C</div>
        <div class="key white" data-key="V" data-freq="349.23">V</div>
        <div class="key black" data-key="G" data-freq="369.99" style="left: 200px;">G</div>
        <div class="key white" data-key="B" data-freq="392.00">B</div>
        <div class="key black" data-key="H" data-freq="415.30" style="left: 250px;">H</div>
        <div class="key white" data-key="N" data-freq="440.00">N</div>
        <div class="key black" data-key="J" data-freq="466.16" style="left: 300px;">J</div>
        <div class="key white" data-key="M" data-freq="493.88">M</div>

        <div class="key white" data-key="Q" data-freq="523.25">Q</div>
        <div class="key black" data-key="2" data-freq="554.37" style="left: 400px;">2</div>
        <div class="key white" data-key="W" data-freq="587.33">W</div>
        <div class="key black" data-key="3" data-freq="622.25" style="left: 450px;">3</div>
        <div class="key white" data-key="E" data-freq="659.25">E</div>
        <div class="key white" data-key="R" data-freq="698.46">R</div>
        <div class="key black" data-key="5" data-freq="739.99" style="left: 550px;">5</div>
        <div class="key white" data-key="T" data-freq="783.99">T</div>
        <div class="key black" data-key="6" data-freq="830.61" style="left: 600px;">6</div>
        <div class="key white" data-key="Y" data-freq="880.00">Y</div>
        <div class="key black" data-key="7" data-freq="932.33" style="left: 650px;">7</div>
        <div class="key white" data-key="U" data-freq="987.77">U</div>
        <div class="key white" data-key="I" data-freq="1046.50">I</div>
    </div>
</div>

<script>
    const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    const activeNotes = new Map();

    function playNote(key, freq) {
        if (activeNotes.has(key)) return;

        const osc = audioCtx.createOscillator();
        const gain = audioCtx.createGain();

        osc.type = 'sine';
        osc.frequency.setValueAtTime(freq, audioCtx.currentTime);

        gain.gain.setValueAtTime(0.2, audioCtx.currentTime);
        gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 1);

        osc.connect(gain);
        gain.connect(audioCtx.destination);

        osc.start();
        activeNotes.set(key, { osc, gain });

        const el = document.querySelector(`[data-key="${key}"]`);
        if (el) el.classList.add('active');
    }

    function stopNote(key) {
        const note = activeNotes.get(key);
        if (note) {
            note.gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.1);
            note.osc.stop(audioCtx.currentTime + 0.1);
            activeNotes.delete(key);
            
            const el = document.querySelector(`[data-key="${key}"]`);
            if (el) el.classList.remove('active');
        }
    }

    // Klavye Dinleme
    window.addEventListener('keydown', e => {
        const key = e.key.toUpperCase();
        const el = document.querySelector(`[data-key="${key}"]`);
        if (el) playNote(key, parseFloat(el.dataset.freq));
    });

    window.addEventListener('keyup', e => {
        stopNote(e.key.toUpperCase());
    });

    // Fare Dinleme
    document.querySelectorAll('.key').forEach(k => {
        k.addEventListener('mousedown', () => playNote(k.dataset.key, parseFloat(k.dataset.freq)));
        k.addEventListener('mouseup', () => stopNote(k.dataset.key));
        k.addEventListener('mouseleave', () => stopNote(k.dataset.key));
    });
</script>

</body>
</html>
