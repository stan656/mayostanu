<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>For Myraa 💗</title>
    <link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&family=Quicksand:wght@300;500&display=swap" rel="stylesheet">
    <style>
        :root { --accent: #ff85a2; --soft-pink: #ffb3c1; --dark-blue: #0a0a1a; }
        body { margin: 0; font-family: 'Quicksand', sans-serif; background: #0a0a12; color: white; min-height: 100vh; overflow-x: hidden; transition: background 3s ease; }
        
        canvas#stars { position: fixed; inset: 0; z-index: 0; pointer-events: none; }
        canvas#fireworks { position: fixed; inset: 0; z-index: 5; pointer-events: none; }

        #lock { position: fixed; inset: 0; background: #1a1a2e; display: flex; flex-direction: column; justify-content: center; align-items: center; z-index: 9999; transition: 1s ease; }
        #lock.hidden { transform: translateY(-100%); opacity: 0; }
        #lock h1 { font-family: 'Dancing Script'; font-size: 3.5rem; color: var(--accent); }
        input { background: rgba(255,255,255,0.05); border: 1px solid var(--accent); padding: 15px; border-radius: 30px; color: white; width: 280px; text-align: center; outline: none; margin-bottom: 20px; }
        
        .container { position: relative; z-index: 10; max-width: 1100px; margin: auto; padding: 40px 20px; text-align: center; }
        header h1 { font-family: 'Dancing Script'; font-size: 4.5rem; color: var(--accent); }
        .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 25px; margin-top: 30px; }
        .card { background: rgba(255, 255, 255, 0.03); border: 1px solid rgba(255, 255, 255, 0.1); padding: 35px 20px; border-radius: 25px; cursor: pointer; transition: 0.4s; backdrop-filter: blur(5px); }
        .card:hover { border-color: var(--accent); transform: translateY(-10px); }

        #modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.85); justify-content: center; align-items: center; z-index: 1000; padding: 20px; }
        .modal-box { background: white; color: #1a1a2e; padding: 40px; border-radius: 30px; max-width: 750px; width: 100%; max-height: 85vh; overflow-y: auto; position: relative; text-align: center; z-index: 1001; }
        #text { line-height: 1.8; font-size: 1.1rem; white-space: pre-wrap; text-align: left; color: #333; }

        #proposal-btns { display: none; justify-content: center; gap: 20px; margin-top: 25px; }
        .choice-btn { padding: 15px 45px; border-radius: 30px; border: none; font-weight: bold; cursor: pointer; font-size: 1.2rem; transition: 0.3s; }
        #yes-btn { background: #ff4d6d; color: white; box-shadow: 0 0 25px rgba(255, 77, 109, 0.6); }
        
        /* Orchids */
        .orchid { position: fixed; font-size: 2.5rem; pointer-events: none; z-index: 2000; animation: orchidFall 6s linear forwards; }
        @keyframes orchidFall {
            0% { transform: translateY(-10vh) rotate(0deg); opacity: 0; }
            10% { opacity: 1; }
            100% { transform: translateY(110vh) rotate(360deg); opacity: 0; }
        }

        .night-sky-bg { background: radial-gradient(circle at center, #1b2735 0%, #090a0f 100%) !important; }
        .gallery-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-top: 25px; }
        .gallery-img { width: 100%; height: 250px; object-fit: cover; border-radius: 15px; border: 4px solid var(--accent); }
        .main-img { width: 100%; border-radius: 20px; border: 6px solid #ff4d6d; margin-top: 20px; }
    </style>
</head>
<body>

<canvas id="stars"></canvas>
<canvas id="fireworks"></canvas>

<audio id="bgMusic" loop><source src="mayo.mp3" type="audio/mpeg"></audio>

<div id="lock">
    <h1>Hi Myraa 💗</h1>
    <input type="password" id="pass" placeholder="Enter Our Secret Password">
    <button class="btn" onclick="unlock()" style="background:var(--accent); color:white; padding:12px 40px; border-radius:30px; border:none; cursor:pointer;">Unlock My Heart</button>
</div>

<div class="container">
    <header><h1>Meri Chaand 🌙</h1></header>
    <div class="grid" id="grid"></div>
</div>

<div id="modal">
    <div class="modal-box" id="modalBox">
        <h2 id="modalTitle" style="font-family:'Dancing Script'; color:#be185d; font-size: 2.5rem;"></h2>
        <div id="text"></div>
        <div id="proposal-btns">
            <button id="yes-btn" class="choice-btn" onclick="finalCelebration()">YES! ❤️</button>
            <button id="no-btn" class="choice-btn" onmouseover="dodgeNo()">No</button>
        </div>
        <div id="cake-celebration" style="display:none;">
            <h2 style="font-family:'Dancing Script'; color:#ff4d6d; font-size:3rem;">FOREVER MINE! ✨</h2>
            <div class="gallery-grid">
                <img src="pic1.jpg" class="gallery-img">
                <img src="pic2.jpg" class="gallery-img">
                <img src="pic3.jpg" class="gallery-img">
                <img src="pic4.jpg" class="gallery-img">
            </div>
            <img src="facetime.jpg" class="main-img">
        </div>
    </div>
</div>

<script>
    const password = "stanlovesmyraamore";
    const letters = [
        "Day 1 Letter Here...", "Day 2 Letter Here...", "Day 3 Letter Here...", 
        "Day 4 Letter Here...", "Day 5 Letter Here...", "Day 6 Letter Here...", 
        "Day 7 Letter Here...", "Day 8 Letter Here...", "Day 9 Letter Here...", 
        "Day 10 Letter Here...", 
        "Day 11 – Valentine’s Promise\n\nHi my baby... (Your full long message goes here)... will you be mine forever?"
    ];

    function unlock() {
        if(document.getElementById("pass").value === password) {
            document.getElementById("lock").classList.add("hidden");
            document.getElementById("bgMusic").play();
            initGrid();
        } else { alert("Wrong password, Jaanu."); }
    }

    function initGrid() {
        const grid = document.getElementById("grid");
        letters.forEach((_, i) => {
            const card = document.createElement("div");
            card.className = "card";
            card.innerHTML = `<h3>Day ${i+1}</h3><p>Open 💌</p>`;
            card.onclick = () => openModal(i);
            grid.appendChild(card);
        });
    }

    function openModal(index) {
        document.getElementById("modal").style.display = "flex";
        document.getElementById("modalTitle").innerText = "Day " + (index + 1);
        const textEl = document.getElementById("text");
        textEl.innerText = "";
        
        if(index === 10) {
            document.body.classList.add("night-sky-bg");
            startFireworks(); // Fireworks start crackling in BG during the proposal
        }

        let i = 0;
        function type() {
            if (i < letters[index].length) {
                textEl.innerText += letters[index].charAt(i); i++;
                setTimeout(type, 20);
            } else if (index === 10) {
                document.getElementById("proposal-btns").style.display = "flex";
            }
        }
        type();
    }

    function finalCelebration() {
        document.getElementById("text").style.display = "none";
        document.getElementById("proposal-btns").style.display = "none";
        document.getElementById("cake-celebration").style.display = "block";
        
        // Flower Rain starts only after YES
        for(let i=0; i<70; i++) {
            setTimeout(() => {
                const o = document.createElement("div");
                o.className = "orchid";
                o.innerHTML = (Math.random() > 0.5) ? "🌸" : "🌹";
                o.style.left = Math.random() * 100 + "vw";
                o.style.animationDuration = (Math.random() * 3 + 4) + "s";
                document.body.appendChild(o);
                setTimeout(() => o.remove(), 6000);
            }, i * 100);
        }
    }

    // Firework System
    const fwCanvas = document.getElementById('fireworks');
    const fwCtx = fwCanvas.getContext('2d');
    fwCanvas.width = window.innerWidth; fwCanvas.height = window.innerHeight;
    let particles = [];

    function createFirework(x, y) {
        const colors = ['#ff0055', '#ff85a2', '#ffffff', '#ffd700', '#00ffcc'];
        const color = colors[Math.floor(Math.random() * colors.length)];
        for(let i=0; i<40; i++) {
            particles.push({ x, y, vx: (Math.random() - 0.5) * 10, vy: (Math.random() - 0.5) * 10, life: 1, color });
        }
    }

    function startFireworks() {
        setInterval(() => { createFirework(Math.random() * fwCanvas.width, Math.random() * (fwCanvas.height/2)); }, 600);
        function update() {
            fwCtx.clearRect(0,0,fwCanvas.width, fwCanvas.height);
            particles.forEach((p, i) => {
                p.x += p.vx; p.y += p.vy; p.vy += 0.1; p.life -= 0.015;
                fwCtx.fillStyle = p.color; fwCtx.globalAlpha = p.life;
                fwCtx.beginPath(); fwCtx.arc(p.x, p.y, 3, 0, Math.PI*2); fwCtx.fill();
                if(p.life <= 0) particles.splice(i, 1);
            });
            requestAnimationFrame(update);
        }
        update();
    }

    function dodgeNo() {
        const btn = document.getElementById("no-btn");
        btn.style.position = "fixed";
        btn.style.left = Math.random() * 80 + "vw";
        btn.style.top = Math.random() * 80 + "vh";
    }

    // Star Field
    const canvas = document.getElementById('stars');
    const ctx = canvas.getContext('2d');
    canvas.width = window.innerWidth; canvas.height = window.innerHeight;
    const sArr = Array.from({length: 100}, () => ({ x: Math.random()*canvas.width, y: Math.random()*canvas.height, s: Math.random()*1.5 }));
    function drawStars() {
        ctx.clearRect(0,0,canvas.width,canvas.height); ctx.fillStyle = "white";
        sArr.forEach(st => { ctx.beginPath(); ctx.arc(st.x, st.y, st.s, 0, Math.PI*2); ctx.fill(); });
        requestAnimationFrame(drawStars);
    }
    drawStars();
</script>
</body>
</html>
