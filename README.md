<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Message du jour (Preview)</title>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700&family=Montserrat:wght@400;600;700&family=Pacifico&display=swap" rel="stylesheet">
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <style>
        :root{
            --bg-1: #ffeef6;
            --bg-2: #f9f0e9;
            --accent-pink: #e86f9c;
            --accent-pink-soft: #ffd8ea;
            --muted: #6b4a5a;
            --card-bg: rgba(255,255,255,0.96);
            --glass: rgba(255,255,255,0.7);
            --shadow: rgba(210,150,180,0.18);
        }
        html,body{height:100%;margin:0;}
        body {
            min-height: 100vh;
            margin: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            background: radial-gradient(1200px 700px at 10% 10%, var(--bg-1) 0%, var(--bg-2) 45%, #e6f3fb 100%);
            font-family: 'Montserrat', Arial, sans-serif;
            -webkit-font-smoothing:antialiased;
            -moz-osx-font-smoothing:grayscale;
            overflow: hidden;
        }
        /* soft decorative shapes behind */
        .decor {
            position:fixed;
            inset:0;
            pointer-events:none;
            z-index:0;
        }
        .blob {
            position:absolute;
            width:420px;height:420px;
            border-radius:50%;
            filter: blur(44px) saturate(115%);
            opacity:0.16;
            transform:translate3d(0,0,0);
        }
        .blob.b1{ left:-8%; top:4%; background: linear-gradient(135deg,#ffd5ea,#ffd9b6);}
        .blob.b2{ right:-6%; bottom:-4%; width:520px;height:520px; background: linear-gradient(135deg,#c6eaf1,#f3e8ff); opacity:0.12; }
        .bubble-emoji {
            position:absolute;
            font-size:3.6rem;
            opacity:0.06;
            transform:rotate(-8deg);
            filter: drop-shadow(0 6px 14px rgba(0,0,0,0.02));
        }
        .bubble-emoji.e1{ left:10%; top:22%; }
        .bubble-emoji.e2{ right:12%; top:28%; font-size:2.8rem; }
        .card-wrap{
            position:relative;
            z-index:2;
            padding:18px;
            /* animated gradient border ring */
        }
        .card {
            background: var(--card-bg);
            border-radius: 26px;
            padding: 40px 34px 28px 34px;
            max-width: 520px;
            width: calc(100% - 36px);
            text-align: center;
            box-shadow: 0 20px 60px var(--shadow), 0 6px 18px rgba(0,0,0,0.04);
            border: 1px solid rgba(255,255,255,0.6);
            position: relative;
            overflow: visible;
            backdrop-filter: blur(6px) saturate(120%);
        }
        /* animated ring */
        .card-wrap::before{
            content:"";
            position:absolute;
            inset:-8px;
            border-radius:30px;
            padding:1px;
            background: conic-gradient(from 120deg, rgba(232,111,156,0.15), rgba(249,226,224,0.12), rgba(129,207,224,0.12), rgba(232,111,156,0.15));
            -webkit-mask: linear-gradient(#000 0 0) content-box, linear-gradient(#000 0 0);
            -webkit-mask-composite: xor;
            mask-composite: exclude;
            animation: spin 8s linear infinite;
            pointer-events:none;
        }
        @keyframes spin{ to{ transform: rotate(360deg); } }
        .header {
            font-family: 'Playfair Display', serif;
            font-size: 1.95rem;
            color: var(--accent-pink);
            margin-bottom: 6px;
            letter-spacing: 0.02em;
            position: relative;
            z-index: 3;
            text-shadow: 0 6px 20px rgba(255,226,242,0.55);
            font-weight:700;
        }
        .meta {
            display:flex;
            align-items:center;
            justify-content:center;
            gap:10px;
            color:var(--muted);
            font-size:0.89rem;
            margin-bottom:14px;
            font-weight:600;
        }
        .day-badge{
            background: linear-gradient(90deg, rgba(232,111,156,0.12), rgba(129,207,224,0.08));
            padding:6px 10px;
            border-radius:999px;
            color:var(--accent-pink);
            font-weight:700;
            box-shadow: 0 4px 14px rgba(232,111,156,0.06);
            font-size:0.95rem;
        }
        #message {
            font-size: 1.18rem;
            color: #8b2f52;
            margin: 10px 2px 6px 2px;
            line-height: 1.7;
            font-family: 'Montserrat', Arial, sans-serif;
            font-weight:600;
            z-index: 1;
            position: relative;
            min-height: 56px;
            display:inline-block;
            text-align:center;
            white-space:pre-wrap;
        }
        /* typing effect */
        .typing {
            border-right: 2px solid rgba(139,47,82,0.18);
            padding-right:4px;
            animation: caret 1s steps(1) infinite;
        }
        @keyframes caret { 50% { border-color: transparent; } }
        .signature {
            font-family: 'Pacifico', cursive;
            font-size: 1.06rem;
            color: #e86f9c;
            margin-top: 18px;
            opacity: 0.95;
            z-index: 1;
            position: relative;
        }
        .heart {
            font-size: 2.1rem;
            display:inline-block;
            transform:translateY(2px);
            margin-right:8px;
            filter: drop-shadow(0 6px 18px rgba(232,111,156,0.12));
        }

        /* small sparkles */
        .sparkle {
            position:absolute;
            width:20px;height:20px;
            z-index:3;opacity:0.8;
        }
        .sparkle svg { filter: drop-shadow(0 6px 12px rgba(0,0,0,0.04)); opacity:0.9;}

        /* responsive */
        @media (max-width:640px){
            .card { padding: 22px 18px; border-radius:18px; }
            .header { font-size:1.3rem; }
            #message { font-size:0.98rem; min-height:46px; }
            .meta { flex-direction:column; gap:6px; font-size:0.85rem; }
            .blob{ display:none; }
            .card-wrap::before{ inset:-6px; border-radius:22px; }
        }
    </style>
</head>
<body>

<div class="decor" aria-hidden="true">
    <div class="blob b1"></div>
    <div class="blob b2"></div>
    <div class="bubble-emoji e1">🏀</div>
    <div class="bubble-emoji e2">💗</div>
</div>

<div class="card-wrap">
    <div class="card" role="article" aria-labelledby="title">
        <div class="header" id="title"><span class="heart">💗</span>Message du jour</div>

        <div class="meta" aria-hidden="false">
            <div id="dayLabel" class="day-badge" style="display:none">Jour 1</div>
            <div id="dateLabel" style="color:var(--muted);font-weight:600;">&nbsp;</div>
        </div>

        <div id="message" aria-live="polite">Chargement...</div>

        <div class="signature" aria-hidden="true">Je t'aimeeeeeeee</div>

        <!-- sparkles -->
        <span class="sparkle" style="top:12px;left:18px;">
            <svg height="20" width="20"><polygon points="10,0 12,8 20,10 12,12 10,20 8,12 0,10 8,8" fill="#ffe0f0"/></svg>
        </span>
        <span class="sparkle" style="bottom:18px;right:26px;">
            <svg height="20" width="20"><polygon points="10,0 12,8 20,10 12,12 10,20 8,12 0,10 8,8" fill="#e6f7fb"/></svg>
        </span>
    </div>
</div>

<script>
    const messages = [
      "Jour 1 : Coucou toi, j’espère que ta journée commence bien. 🌸",
      "Jour 2 : Tu es le meilleur. 😊🌸",
      "Jour 3 : Merci d’être là, même de loin. 💌",
      "Jour 4 : J’aime trop nos discussions, elles me rendent trop heureuse. ✨",
      "Jour 5 : Tu me manques. 🐻",
      "Jour 6 : Je t'aime. 💪🧸",
      "Jour 7 : Franchement, je suis contente que ce soit toi. 🥰",
      "Jour 8 : Je te le dis pas souvent mais… je t’aime 🫶💗",
      "Jour 9 : Merci d'être toi. 💖",
      "Jour 10 : Continue comme tu es, tu gères. 🌟",
      "Jour 11 : Merci de me respecter comme tu le fais. 🙏",
      "Jour 12 : Je suis toujours contente de recevoir ton message. 🤗",
      "Jour 13 : T’es quelqu’un de bien, c’est rare. 💫",
      "Jour 14 : J’ai hâte qu’on se revoie. 🧁",
      "Jour 15 : Toi + moi = bonne décision 😌💕",
      "Jour 16 : Je suis contente de t’avoir rencontré. 🌷",
      "Jour 17 : J’aime notre petit truc à nous. 🫶",
      "Jour 18 : Ça me fait plaisir de te connaître. 🍓",
      "Jour 19 : J’aime trop la façon dont on est ensemble. 🦋",
      "Jour 20 : Repose-toi des fois, tu le mérites. 🌙",
      "Jour 21 : Même de loin, je suis là pour toi.",
      "Jour 22 : Pardonne moi d'être trop énervante des fois",
      "Jour 23 : Tu es celui avec qui je veux finir ma vie.",
      "Jour 24 : Je t'aime.",
      "Jour 25 : Merci de me donner une place dans ta vie. 🍰",
      "Jour 26 : Si tu savais comme je tiens à toi. 💕",
      "Jour 27 : Joyeux 5 mois l'amour de ma vie. Je t'aime à un point inimaginable💖"
    ];

    // Preview start date set to current date so the page shows the first message immediately.
    const startDate = new Date("2025-12-27");
    const today = new Date();
    const startDateUTC = Date.UTC(startDate.getFullYear(), startDate.getMonth(), startDate.getDate());
    const todayUTC = Date.UTC(today.getFullYear(), today.getMonth(), today.getDate());
    const diffDays = Math.floor((todayUTC - startDateUTC) / (1000 * 60 * 60 * 24));

    const messageDiv = document.getElementById("message");
    const dayLabel = document.getElementById("dayLabel");
    const dateLabel = document.getElementById("dateLabel");

    function setMeta(index){
        // index is 0-based day index
        const dayNumber = index + 1;
        dayLabel.textContent = `Jour ${dayNumber}`;
        dayLabel.style.display = "inline-block";
        // display the actual date for that day
        const d = new Date(startDate.getFullYear(), startDate.getMonth(), startDate.getDate() + index);
        dateLabel.textContent = d.toLocaleDateString(undefined, { weekday: 'short', day: '2-digit', month: 'short' });
    }

    function typeText(node, text, speed=24){
        // respect reduced-motion
        const prefersReduced = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
        if(prefersReduced){
            node.textContent = text;
            return;
        }
        node.classList.add('typing');
        node.textContent = "";
        let i=0;
        const timer = setInterval(()=>{
            node.textContent += text.charAt(i);
            i++;
            if(i>=text.length){
                clearInterval(timer);
            <!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Message du jour (Preview)</title>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700&family=Montserrat:wght@400;600;700&family=Pacifico&display=swap" rel="stylesheet">
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <style>
        :root{
            --bg-1: #ffeef6;
            --bg-2: #f9f0e9;
            --accent-pink: #e86f9c;
            --accent-pink-soft: #ffd8ea;
            --muted: #6b4a5a;
            --card-bg: rgba(255,255,255,0.96);
            --glass: rgba(255,255,255,0.7);
            --shadow: rgba(210,150,180,0.18);
        }
        html,body{height:100%;margin:0;}
        body {
            min-height: 100vh;
            margin: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            background: radial-gradient(1200px 700px at 10% 10%, var(--bg-1) 0%, var(--bg-2) 45%, #e6f3fb 100%);
            font-family: 'Montserrat', Arial, sans-serif;
            -webkit-font-smoothing:antialiased;
            -moz-osx-font-smoothing:grayscale;
            overflow: hidden;
        }
        /* soft decorative shapes behind */
        .decor {
            position:fixed;
            inset:0;
            pointer-events:none;
            z-index:0;
        }
        .blob {
            position:absolute;
            width:420px;height:420px;
            border-radius:50%;
            filter: blur(44px) saturate(115%);
            opacity:0.16;
            transform:translate3d(0,0,0);
        }
        .blob.b1{ left:-8%; top:4%; background: linear-gradient(135deg,#ffd5ea,#ffd9b6);}
        .blob.b2{ right:-6%; bottom:-4%; width:520px;height:520px; background: linear-gradient(135deg,#c6eaf1,#f3e8ff); opacity:0.12; }
        .bubble-emoji {
            position:absolute;
            font-size:3.6rem;
            opacity:0.06;
            transform:rotate(-8deg);
            filter: drop-shadow(0 6px 14px rgba(0,0,0,0.02));
        }
        .bubble-emoji.e1{ left:10%; top:22%; }
        .bubble-emoji.e2{ right:12%; top:28%; font-size:2.8rem; }
        .card-wrap{
            position:relative;
            z-index:2;
            padding:18px;
            /* animated gradient border ring */
        }
        .card {
            background: var(--card-bg);
            border-radius: 26px;
            padding: 40px 34px 28px 34px;
            max-width: 520px;
            width: calc(100% - 36px);
            text-align: center;
            box-shadow: 0 20px 60px var(--shadow), 0 6px 18px rgba(0,0,0,0.04);
            border: 1px solid rgba(255,255,255,0.6);
            position: relative;
            overflow: visible;
            backdrop-filter: blur(6px) saturate(120%);
        }
        /* animated ring */
        .card-wrap::before{
            content:"";
            position:absolute;
            inset:-8px;
            border-radius:30px;
            padding:1px;
            background: conic-gradient(from 120deg, rgba(232,111,156,0.15), rgba(249,226,224,0.12), rgba(129,207,224,0.12), rgba(232,111,156,0.15));
            -webkit-mask: linear-gradient(#000 0 0) content-box, linear-gradient(#000 0 0);
            -webkit-mask-composite: xor;
            mask-composite: exclude;
            animation: spin 8s linear infinite;
            pointer-events:none;
        }
        @keyframes spin{ to{ transform: rotate(360deg); } }
        .header {
            font-family: 'Playfair Display', serif;
            font-size: 1.95rem;
            color: var(--accent-pink);
            margin-bottom: 6px;
            letter-spacing: 0.02em;
            position: relative;
            z-index: 3;
            text-shadow: 0 6px 20px rgba(255,226,242,0.55);
            font-weight:700;
        }
        .meta {
            display:flex;
            align-items:center;
            justify-content:center;
            gap:10px;
            color:var(--muted);
            font-size:0.89rem;
            margin-bottom:14px;
            font-weight:600;
        }
        .day-badge{
            background: linear-gradient(90deg, rgba(232,111,156,0.12), rgba(129,207,224,0.08));
            padding:6px 10px;
            border-radius:999px;
            color:var(--accent-pink);
            font-weight:700;
            box-shadow: 0 4px 14px rgba(232,111,156,0.06);
            font-size:0.95rem;
        }
        #message {
            font-size: 1.18rem;
            color: #8b2f52;
            margin: 10px 2px 6px 2px;
            line-height: 1.7;
            font-family: 'Montserrat', Arial, sans-serif;
            font-weight:600;
            z-index: 1;
            position: relative;
            min-height: 56px;
            display:inline-block;
            text-align:center;
            white-space:pre-wrap;
        }
        /* typing effect */
        .typing {
            border-right: 2px solid rgba(139,47,82,0.18);
            padding-right:4px;
            animation: caret 1s steps(1) infinite;
        }
        @keyframes caret { 50% { border-color: transparent; } }
        .signature {
            font-family: 'Pacifico', cursive;
            font-size: 1.06rem;
            color: #e86f9c;
            margin-top: 18px;
            opacity: 0.95;
            z-index: 1;
            position: relative;
        }
        .heart {
            font-size: 2.1rem;
            display:inline-block;
            transform:translateY(2px);
            margin-right:8px;
            filter: drop-shadow(0 6px 18px rgba(232,111,156,0.12));
        }

        /* small sparkles */
        .sparkle {
            position:absolute;
            width:20px;height:20px;
            z-index:3;opacity:0.8;
        }
        .sparkle svg { filter: drop-shadow(0 6px 12px rgba(0,0,0,0.04)); opacity:0.9;}

        /* responsive */
        @media (max-width:640px){
            .card { padding: 22px 18px; border-radius:18px; }
            .header { font-size:1.3rem; }
            #message { font-size:0.98rem; min-height:46px; }
            .meta { flex-direction:column; gap:6px; font-size:0.85rem; }
            .blob{ display:none; }
            .card-wrap::before{ inset:-6px; border-radius:22px; }
        }
    </style>
</head>
<body>

<div class="decor" aria-hidden="true">
    <div class="blob b1"></div>
    <div class="blob b2"></div>
    <div class="bubble-emoji e1">🏀</div>
    <div class="bubble-emoji e2">💗</div>
</div>

<div class="card-wrap">
    <div class="card" role="article" aria-labelledby="title">
        <div class="header" id="title"><span class="heart">💗</span>Message du jour</div>

        <div class="meta" aria-hidden="false">
            <div id="dayLabel" class="day-badge" style="display:none">Jour 1</div>
            <div id="dateLabel" style="color:var(--muted);font-weight:600;">&nbsp;</div>
        </div>

        <div id="message" aria-live="polite">Chargement...</div>

        <div class="signature" aria-hidden="true">Je t'aimeeeeeeee</div>

        <!-- sparkles -->
        <span class="sparkle" style="top:12px;left:18px;">
            <svg height="20" width="20"><polygon points="10,0 12,8 20,10 12,12 10,20 8,12 0,10 8,8" fill="#ffe0f0"/></svg>
        </span>
        <span class="sparkle" style="bottom:18px;right:26px;">
            <svg height="20" width="20"><polygon points="10,0 12,8 20,10 12,12 10,20 8,12 0,10 8,8" fill="#e6f7fb"/></svg>
        </span>
    </div>
</div>

<script>
    const messages = [
      "Jour 1 : Coucou toi, j’espère que ta journée commence bien. 🌸",
      "Jour 2 : Tu es le meilleur. 😊🌸",
      "Jour 3 : Merci d’être là, même de loin. 💌",
      "Jour 4 : J’aime trop nos discussions, elles me rendent trop heureuse. ✨",
      "Jour 5 : Tu me manques. 🐻",
      "Jour 6 : Je t'aime. 💪🧸",
      "Jour 7 : Franchement, je suis contente que ce soit toi. 🥰",
      "Jour 8 : Je te le dis pas souvent mais… je t’aime 🫶💗",
      "Jour 9 : Merci d'être toi. 💖",
      "Jour 10 : Continue comme tu es, tu gères. 🌟",
      "Jour 11 : Merci de me respecter comme tu le fais. 🙏",
      "Jour 12 : Je suis toujours contente de recevoir ton message. 🤗",
      "Jour 13 : T’es quelqu’un de bien, c’est rare. 💫",
      "Jour 14 : J’ai hâte qu’on se revoie. 🧁",
      "Jour 15 : Toi + moi = bonne décision 😌💕",
      "Jour 16 : Je suis contente de t’avoir rencontré. 🌷",
      "Jour 17 : J’aime notre petit truc à nous. 🫶",
      "Jour 18 : Ça me fait plaisir de te connaître. 🍓",
      "Jour 19 : J’aime trop la façon dont on est ensemble. 🦋",
      "Jour 20 : Repose-toi des fois, tu le mérites. 🌙",
      "Jour 21 : Même de loin, je suis là pour toi.",
      "Jour 22 : Pardonne moi d'être trop énervante des fois",
      "Jour 23 : Tu es celui avec qui je veux finir ma vie.",
      "Jour 24 : Je t'aime.",
      "Jour 25 : Merci de me donner une place dans ta vie. 🍰",
      "Jour 26 : Si tu savais comme je tiens à toi. 💕",
      "Jour 27 : Joyeux 5 mois l'amour de ma vie. Je t'aime à un point inimaginable💖"
    ];

    // Preview start date set to current date so the page shows the first message immediately.
    const startDate = new Date("2025-12-27");
    const today = new Date();
    const startDateUTC = Date.UTC(startDate.getFullYear(), startDate.getMonth(), startDate.getDate());
    const todayUTC = Date.UTC(today.getFullYear(), today.getMonth(), today.getDate());
    const diffDays = Math.floor((todayUTC - startDateUTC) / (1000 * 60 * 60 * 24));

    const messageDiv = document.getElementById("message");
    const dayLabel = document.getElementById("dayLabel");
    const dateLabel = document.getElementById("dateLabel");

    function setMeta(index){
        // index is 0-based day index
        const dayNumber = index + 1;
        dayLabel.textContent = `Jour ${dayNumber}`;
        dayLabel.style.display = "inline-block";
        // display the actual date for that day
        const d = new Date(startDate.getFullYear(), startDate.getMonth(), startDate.getDate() + index);
        dateLabel.textContent = d.toLocaleDateString(undefined, { weekday: 'short', day: '2-digit', month: 'short' });
    }

    function typeText(node, text, speed=24){
        // respect reduced-motion
        const prefersReduced = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
        if(prefersReduced){
            node.textContent = text;
            return;
        }
        node.classList.add('typing');
        node.textContent = "";
        let i=0;
        const timer = setInterval(()=>{
            node.textContent += text.charAt(i);
            i++;
            if(i>=text.length){
                clearInterval(timer);
                node.classList.remove('typing');
            }
        }, speed);
    }

    if(diffDays < 0) {
        messageDiv.textContent = "Patiente un peu… Les plus belles choses méritent d’attendre. 💖🍬";
        dateLabel.textContent = startDate.toLocaleDateString(undefined, { weekday: 'short', day: '2-digit', month: 'short', year:'numeric' });
    } else {
        let index = diffDays;
        if(index >= messages.length) index = messages.length - 1;
        // show meta
        setMeta(index);
        // typing effect for the message
        typeText(messageDiv, messages[index], 26);
    }
</script>

</body>
</html>    node.classList.remove('typing');
            }
        }, speed);
    }

    if(diffDays < 0) {
        messageDiv.textContent = "Patiente un peu… Les plus belles choses méritent d’attendre. 💖🍬";
        dateLabel.textContent = startDate.toLocaleDateString(undefined, { weekday: 'short', day: '2-digit', month: 'short', year:'numeric' });
    } else {
        let index = diffDays;
        if(index >= messages.length) index = messages.length - 1;
        // show meta
        setMeta(index);
        // typing effect for the message
        typeText(messageDiv, messages[index], 26);
    }
</script>

</body>
</html>
