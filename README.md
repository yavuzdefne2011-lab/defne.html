<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <title>Engellerden Kaçış: Araba Oyunu</title>
    <style>
        body {
            margin: 0; background: #222; overflow: hidden;
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            height: 100vh; font-family: 'Arial', sans-serif; color: white;
        }
        canvas {
            background: #444; border: 4px solid #fff; border-radius: 5px;
            box-shadow: 0 0 20px rgba(0,0,0,0.5);
        }
        .ui { position: absolute; top: 20px; text-align: center; pointer-events: none; }
        #gameOver { 
            display: none; position: absolute; top: 50%; left: 50%;
            transform: translate(-50%, -50%); background: rgba(0,0,0,0.8);
            padding: 20px; border-radius: 10px; text-align: center; z-index: 10;
        }
    </style>
</head>
<body>

<div class="ui">
    <h1>YOLDA KAL!</h1>
    <p>Skor: <span id="score">0</span> | Hız: <span id="speed">1</span>x</p>
</div>

<div id="gameOver">
    <h1>OYUN BİTTİ!</h1>
    <p>Skorun: <span id="finalScore">0</span></p>
    <button onclick="location.reload()" style="padding: 10px 20px; cursor: pointer;">Tekrar Dene</button>
</div>

<canvas id="gameCanvas"></canvas>

<script>
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');
    const scoreEl = document.getElementById('score');
    const speedEl = document.getElementById('speed');

    canvas.width = 400;
    canvas.height = 600;

    // Oyun Değişkenleri
    let score = 0;
    let gameSpeed = 5;
    let gameActive = true;
    const keys = {};

    // Araba Nesnesi
    const player = {
        x: canvas.width / 2 - 20,
        y: canvas.height - 100,
        width: 40,
        height: 70,
        speed: 6,
        color: '#3498db'
    };
