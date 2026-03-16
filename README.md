DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nuestro Refugio Secreto</title>
    <style>
        :root {
            --primary: #ff4d94;
            --secondary: #ff85a2;
            --accent: #ffd1dc;
            --text: #2d1b24;
            --glass: rgba(255, 255, 255, 0.7);
        }

        * { box-sizing: border-box; margin: 0; padding: 0; }

        body {
            font-family: 'Quicksand', 'Segoe UI', sans-serif;
            background: linear-gradient(135deg, #fff5f7 0%, #ffe4ec 100%);
            color: var(--text);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            overflow-x: hidden;
            transition: background 1.5s ease;
        }

        /* --- PANTALLA DE BLOQUEO (ELEGANTE) --- */
        #lockScreen {
            position: fixed; inset: 0;
            background: linear-gradient(135deg, #ffd6e7 0%, #ffffff 100%);
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            z-index: 10000; transition: all 1s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .lock-box {
            background: var(--glass);
            backdrop-filter: blur(15px);
            padding: 50px; border-radius: 40px;
            box-shadow: 0 25px 50px rgba(0,0,0,0.1);
            text-align: center; border: 1px solid white;
            animation: float 4s infinite ease-in-out;
        }

        .lock-box h2 { color: var(--primary); margin-bottom: 10px; font-size: 26px; }

        #pass {
            padding: 15px; width: 100%; border-radius: 20px; border: 1px solid var(--accent);
            margin: 20px 0; text-align: center; font-size: 18px; outline: none;
            box-shadow: inset 0 2px 5px rgba(0,0,0,0.05);
        }

        .unlock-btn {
            background: var(--primary); color: white; border: none;
            padding: 15px 40px; border-radius: 20px; font-weight: bold;
            cursor: pointer; font-size: 16px; transition: 0.3s;
            box-shadow: 0 10px 20px rgba(255, 77, 148, 0.3);
        }
        .unlock-btn:hover { transform: scale(1.05); background: #e63980; }

        /* --- HEADER --- */
        header { padding: 60px 20px; opacity: 0; transform: translateY(-20px); transition: 1s 0.5s; }
        h1 { font-size: 3.5rem; color: var(--primary); letter-spacing: -2px; }
        .subtitle { font-size: 1.2rem; color: var(--secondary); margin-top: 10px; }

        /* --- CONTENEDOR DE CARTAS --- */
        .container {
            display: flex; flex-wrap: wrap; justify-content: center;
            gap: 30px; padding: 20px; max-width: 1200px; margin: 0 auto;
            opacity: 0; transition: 1s 0.8s;
        }

        .card {
            width: 320px; background: var(--glass); backdrop-filter: blur(10px);
            padding: 40px; border-radius: 35px; border: 1px solid white;
            cursor: pointer; transition: 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            box-shadow: 0 15px 35px rgba(0,0,0,0.05);
            display: flex; flex-direction: column; align-items: center;
        }
        .card:hover { transform: translateY(-15px); background: white; box-shadow: 0 25px 50px rgba(255, 77, 148, 0.15); }
        .card-icon { font-size: 50px; margin-bottom: 20px; }
        .card-text { font-weight: 800; font-size: 1.2rem; color: var(--primary); }

        /* --- CARTAS (CONTENIDO PROFUNDO) --- */
        .letter {
            display: none; max-width: 800px; margin: 40px auto;
            background: white; padding: 60px; border-radius: 50px;
            box-shadow: 0 30px 60px rgba(0,0,0,0.1);
            animation: letterSlide 0.8s ease-out;
            position: relative; z-index: 100;
        }

        @keyframes letterSlide { from { opacity:0; transform: translateY(100px); } to { opacity:1; transform: translateY(0); } }

        .letter h2 { font-size: 2.5rem; margin-bottom: 30px; color: var(--primary); text-align: center; }
        .letter p { font-size: 1.2rem; line-height: 2; color: #444; }

        .back-btn {
            display: block; margin: 40px auto 0; padding: 15px 40px;
            border: none; border-radius: 20px; background: #f0f0f0;
            color: #666; font-weight: bold; cursor: pointer; transition: 0.3s;
        }
        .back-btn:hover { background: var(--accent); color: var(--primary); }

        /* --- MODOS DE FONDO --- */
        .mode-sad { background: linear-gradient(135deg, #e0f2fe 0%, #f0f9ff 100%); }
        .mode-miss { background: linear-gradient(135deg, #fce7f3 0%, #fff1f2 100%); }
        .mode-night { background: linear-gradient(135deg, #0f172a 0%, #1e293b 100%); color: white !important; }
        .mode-night .letter { background: #1a2235; color: white; border: 1px solid #2d3748; }
        .mode-night .letter p { color: #cbd5e1; }
        .mode-night h1, .mode-night .subtitle { color: white; }

        /* --- ELEMENTOS MÁGICOS --- */
        .heart { position: fixed; pointer-events: none; animation: heartFloat 5s linear forwards; z-index: 1000; }
        @keyframes heartFloat { 
            0% { transform: translateY(100vh) scale(0); opacity: 0; }
            20% { opacity: 0.8; }
            100% { transform: translateY(-10vh) scale(1.5); opacity: 0; }
        }

        .star { position: fixed; background: white; border-radius: 50%; animation: twinkle 1s infinite alternate; }
        @keyframes twinkle { from { opacity: 0.2; } to { opacity: 1; } }

        .btn-msg {
            background: #25d366; color: white; border: none; padding: 15px 30px;
            border-radius: 25px; font-weight: bold; cursor: pointer; display: inline-flex;
            align-items: center; gap: 10px; text-decoration: none; margin-top: 20px;
            transition: 0.3s; box-shadow: 0 10px 20px rgba(37, 211, 102, 0.2);
        }
        .btn-msg:hover { transform: scale(1.05); box-shadow: 0 15px 30px rgba(37, 211, 102, 0.3); }

        @keyframes float { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-20px); } }

        /* --- RESPONSIVO --- */
        @media (max-width: 600px) {
            h1 { font-size: 2.2rem; }
            .letter { padding: 30px; margin: 15px; }
            .letter h2 { font-size: 1.8rem; }
            .card { width: 100%; }
        }
    </style>
</head>
<body>

<div id="lockScreen">
    <div class="lock-box">
        <h2>Refugio Privado ☁️</h2>
        <p>Solo para nosotros dos</p>
        <input type="password" id="pass" placeholder="Palabra secreta">
        <button class="unlock-btn" onclick="unlock()">Entrar al rincón</button>
    </div>
</div>

<header id="header">
    <h1>Nuestro Rincón 🌸</h1>
    <p class="subtitle">La distancia es solo una prueba de qué tan lejos puede viajar un beso</p>
    <a href="https://wa.me/573000000000" target="_blank" class="btn-msg">
        <span>💬 Hablar ahora mismo</span>
    </a>
</header>

<div class="container" id="menu">
    <div class="card" onclick="openLetter('carta1')">
        <div class="card-icon">🌧️</div>
        <div class="card-text">Si el día está gris</div>
    </div>
    <div class="card" onclick="openLetter('carta2')">
        <div class="card-icon">🫂</div>
        <div class="card-text">Si me extrañas mucho</div>
    </div>
    <div class="card" onclick="openLetter('carta3')">
        <div class="card-icon">🌙</div>
        <div class="card-text">Si no puedes dormir</div>
    </div>
</div>

<div class="letter" id="carta1">
    <h2>Para tus días grises ❤️</h2>
    <p>
        Mi cielo,<br><br>
        Si abriste esta carta es porque hoy el mundo pesa un poco más de lo normal. Quiero que hagas una pausa, respires profundo y cierres los ojos.<br><br>
        ¿Lo sientes? Es mi abrazo. Un abrazo que no entiende de fronteras, que sale de Colombia y llega directo a ti en Francia para recordarte que <b>eres la persona más valiente y asombrosa</b> que conozco.<br><br>
        Desde que nuestros caminos se cruzaron en Argentina, supe que eras especial. No permitas que un mal día borre tu luz. Recuerda que aquí, al otro lado del mapa, hay alguien que te admira profundamente y que cuenta los días para recordarte en persona lo mucho que vales.<br><br>
        Todo pasará. Quédate un momento en este refugio hasta que te sientas mejor. Te amo.
    </p>
    <button class="back-btn" onclick="goBack()">Volver</button>
</div>

<div class="letter" id="carta2">
    <h2>Estando lejos, te siento cerca 💌</h2>
    <p>
        Amor mío,<br><br>
        La distancia es difícil, no te voy a mentir. Hay días en los que el vacío de no tenerte al lado se siente gigante. Pero quiero que mires el lado hermoso: <b>nos elegimos cada día</b> a pesar de los kilómetros.<br><br>
        Que estemos logrando esto significa que lo nuestro es real, que es fuego y que es eterno. No estamos separados, solo nos estamos preparando para el momento en que ya no tengamos que decir "adiós" por una pantalla.<br><br>
        Cuando sientas que me extrañas demasiado, pon tu mano en tu corazón. Ahí estoy yo, y ahí estaré siempre. Lo nuestro empezó en Argentina como una chispa y hoy es un sol que ilumina mi vida entera.<br><br>
        Falta un día menos para volver a caminar juntos. Te espero siempre.
    </p>
    <button class="back-btn" onclick="goBack()">Volver</button>
</div>

<div class="letter" id="carta3">
    <h2>Bajo el mismo cielo 🌙</h2>
    <p>
        Hola, mi vida...<br><br>
        ¿El silencio de la noche es muy fuerte hoy? No dejes que los pensamientos te quiten el sueño. Imagina que estoy ahí, justo a tu lado, acariciando tu cabello mientras te quedas dormida.<br><br>
        Mira por la ventana: la luna que ves es exactamente la misma que yo veo desde aquí. Estamos conectados por ese hilo de plata. <br><br>
        Descansa, relaja tus hombros y confía. Mañana será un día más cerca de nuestro reencuentro. Te veo en mis sueños, donde no hay aeropuertos ni despedidas.<br><br>
        Buenas noches, amor de mi vida.
    </p>
    <button class="back-btn" onclick="goBack()">Volver</button>
</div>

<script>
    const PALABRA = 'nube';
    let cartasLeidas = new Set();

    function unlock() {
        const p = document.getElementById('pass').value.toLowerCase();
        if(p === PALABRA) {
            const screen = document.getElementById('lockScreen');
            screen.style.opacity = '0';
            screen.style.transform = 'scale(1.5)';
            setTimeout(() => {
                screen.style.display = 'none';
                document.getElementById('header').style.opacity = '1';
                document.getElementById('header').style.transform = 'translateY(0)';
                document.getElementById('menu').style.opacity = '1';
                document.getElementById('menu').style.transform = 'translateY(0)';
            }, 800);
            crearPinguinos();
        } else {
            alert("Esa no es nuestra palabra... 🥺");
        }
    }

    function openLetter(id) {
        document.getElementById('menu').style.display = 'none';
        document.getElementById(id).style.display = 'block';
        cartasLeidas.add(id);

        document.body.className = ''; // reset clases
        if(id === 'carta1') { document.body.classList.add('mode-sad'); }
        if(id === 'carta2') { document.body.classList.add('mode-miss'); }
        if(id === 'carta3') { document.body.classList.add('mode-night'); crearEstrellas(); }
    }

    function goBack() {
        document.querySelectorAll('.letter').forEach(l => l.style.display = 'none');
        document.getElementById('menu').style.display = 'flex';
        document.body.className = '';
        limpiarEstrellas();

        if(cartasLeidas.size === 3) {
            setTimeout(mostrarPremioFinal, 1000);
        }
    }

    function crearPinguinos() {
        for(let i=0; i<15; i++) {
            setTimeout(() => {
                const p = document.createElement('div');
                p.style.position = 'fixed';
                p.style.bottom = '-50px';
                p.style.left = Math.random() * 100 + 'vw';
                p.style.fontSize = '40px';
                p.style.transition = 'all 3s ease-out';
                p.innerHTML = '🐧';
                document.body.appendChild(p);
                setTimeout(() => {
                    p.style.transform = 'translateY(-120vh) rotate('+(Math.random()*360)+'deg)';
                    p.style.opacity = '0';
                }, 100);
                setTimeout(() => p.remove(), 3500);
            }, i * 200);
        }
    }

    function crearEstrellas() {
        for(let i=0; i<100; i++) {
            const s = document.createElement('div');
            s.className = 'star';
            const size = Math.random() * 3 + 'px';
            s.style.width = size;
            s.style.height = size;
            s.style.left = Math.random() * 100 + 'vw';
            s.style.top = Math.random() * 100 + 'vh';
            s.style.animationDelay = Math.random() * 2 + 's';
            document.body.appendChild(s);
        }
    }

    function limpiarEstrellas() {
        document.querySelectorAll('.star').forEach(s => s.remove());
    }

    function mostrarPremioFinal() {
        const overlay = document.createElement('div');
        overlay.style.cssText = 'position:fixed; inset:0; background:rgba(255,77,148,0.95); z-index:5000; display:flex; align-items:center; justify-content:center; animation: fadeIn 1s;';
        
        const box = document.createElement('div');
        box.style.cssText = 'background:white; padding:50px; border-radius:40px; text-align:center; max-width:500px; box-shadow:0 30px 60px rgba(0,0,0,0.3);';
        box.innerHTML = `
            <h2 style="color:#ff4d94; font-size:30px">🎁 Mi Promesa Eterna</h2>
            <p style="margin:20px 0; font-size:18px; line-height:1.6">
                Has leído todo mi corazón hoy. Gracias por estar conmigo a pesar de los kilómetros. <br><br>
                <b>Prometo que este "nos vemos pronto" se convertirá en un "ya no me iré nunca".</b> <br><br>
                Te llevaré a ese lugar que soñamos y brindaremos por lo valientes que fuimos.
            </p>
            <button onclick="this.parentElement.parentElement.remove()" style="padding:15px 30px; border:none; background:#ff4d94; color:white; border-radius:15px; cursor:pointer; font-weight:bold;">Eres el amor de mi vida ❤️</button>
        `;
        overlay.appendChild(box);
        document.body.appendChild(overlay);
        
        // Lluvia de corazones infinita por 5 segundos
        let interval = setInterval(crearCorazon, 200);
        setTimeout(() => clearInterval(interval), 5000);
    }

    function crearCorazon() {
        const h = document.createElement('div');
        h.className = 'heart';
        h.innerHTML = ['❤️','💖','🌸','✨'][Math.floor(Math.random()*4)];
        h.style.left = Math.random() * 100 + 'vw';
        h.style.fontSize = Math.random() * 30 + 10 + 'px';
        document.body.appendChild(h);
        setTimeout(() => h.remove(), 5000);
    }

    setInterval(crearCorazon, 800);
</script>

</body>
</html>
