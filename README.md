Web tarayıcılarında gerçek anlamda 3D bir deneyim yaratmak için en güçlü kütüphane Three.js'dir. Sadece HTML ve CSS ile gerçek bir 3D dünya yapmak imkansıza yakındır, bu yüzden bu kodda Three.js kütüphanesini kullanarak sana hareketli bir çöp adam (karakter) hazırladım.

Bu kod; zemini, ışıklandırmayı ve W-A-S-D tuşlarıyla 3 boyutlu uzayda hareket eden, hareket ederken de "yürüme animasyonu" yapan bir karakteri içerir.

3D Yürüyen Karakter Simülasyonu
Aşağıdaki kodu bir .html dosyası olarak kaydet ve çalıştır:

HTML
<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <title>3D Hareketli Karakter</title>
    <style>
        body { margin: 0; overflow: hidden; background: #111; }
        #ui {
            position: absolute; top: 10px; left: 10px;
            color: white; font-family: sans-serif;
            background: rgba(0,0,0,0.5); padding: 10px; border-radius: 5px;
        }
    </style>
</head>
<body>

<div id="ui">
    Hareket: <b>W, A, S, D</b><br>
    Karakter: 3D Çöp Adam
</div>

<script type="module">
    import * as THREE from 'https://cdn.skypack.dev/three@0.136.0';

    // 1. SAHNE AYARLARI
    const scene = new THREE.Scene();
    scene.background = new THREE.Color(0x222222);
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    const renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.setSize(window.innerWidth, window.innerHeight);
    document.body.appendChild(renderer.domElement);

    // 2. IŞIKLANDIRMA
    const light = new THREE.DirectionalLight(0xffffff, 1);
    light.position.set(5, 10, 7.5);
    scene.add(light);
    scene.add(new THREE.AmbientLight(0x404040));

    // 3. ZEMİN (Izgara)
    const grid = new THREE.GridHelper(100, 50, 0x444444, 0x222222);
    scene.add(grid);

    // 4. KARAKTER OLUŞTURMA (Grup olarak)
    const character = new THREE.Group();
    
    // Gövde
    const bodyGeo = new THREE.BoxGeometry(0.5, 0.8, 0.2);
    const bodyMat = new THREE.MeshPhongMaterial({ color: 0x3498db });
    const body = new THREE.Mesh(bodyGeo, bodyMat);
    body.position.y = 1.2;
    character.add(body);

    // Baş
    const headGeo = new THREE.SphereGeometry(0.25, 16, 16);
    const head = new THREE.Mesh(headGeo, bodyMat);
    head.position.y = 1.8;
    character.add(head);

    // Bacaklar
    const legGeo = new THREE.BoxGeometry(0.15, 0.6, 0.15);
    const leftLeg = new THREE.Mesh(legGeo, bodyMat);
    const rightLeg = new THREE.Mesh(legGeo, bodyMat);
    leftLeg.position.set(-0.15, 0.5, 0);
    rightLeg.position.set(0.15, 0.5, 0);
    character.add(leftLeg, rightLeg);

    scene.add(character);
    camera.position.set(0, 5, 10);
    camera.lookAt(character.position);

    // 5. HAREKET KONTROLLERİ
    const keys = {};
    window.addEventListener('keydown', (e) => keys[e.key.toLowerCase()] = true);
    window.addEventListener('keyup', (e) => keys[e.key.toLowerCase()] = false);

    let step = 0;

    function animate() {
        requestAnimationFrame(animate);

        const moveSpeed = 0.1;
        let isMoving = false;

        if (keys['w']) { character.position.z -= moveSpeed; character.rotation.y = Math.PI; isMoving = true; }
        if (keys['s']) { character.position.z += moveSpeed; character.rotation.y = 0; isMoving = true; }
        if (keys['a']) { character.position.x -= moveSpeed; character.rotation.y = -Math.PI/2; isMoving = true; }
        if (keys['d']) { character.position.x += moveSpeed; character.rotation.y = Math.PI/2; isMoving = true; }

        // Yürüme Animasyonu (Bacakları salla)
        if (isMoving) {
            step += 0.2;
            leftLeg.rotation.x = Math.sin(step) * 0.5;
            rightLeg.rotation.x = Math.cos(step) * 0.5;
        } else {
            leftLeg.rotation.x = 0;
            rightLeg.rotation.x = 0;
        }

        // Kamera Karakteri Takip Etsin
        camera.position.x = character.position.x;
        camera.position.z = character.position.z + 10;
        camera.lookAt(character.position);

        renderer.render(scene, camera);
    }

    animate();

    // Pencere Boyutu Değişirse Uyarla
    window.addEventListener('resize', () => {
        camera.aspect = window.innerWidth / window.innerHeight;
        camera.updateProjectionMatrix();
        renderer.setSize(window.innerWidth, window.innerHeight);
    });
</script>

</body>
</html>
