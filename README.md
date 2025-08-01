<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DaviSlots</title>
    <style>
        body {
            margin: 0;
            font-family: 'Trebuchet MS', sans-serif;
            background: linear-gradient(135deg, #1a1a1a, #0d0d0d);
            color: white;
            text-align: center;
        }
        .container {
            max-width: 400px;
            margin: auto;
            padding: 20px;
        }
        h1, h2 {
            color: gold;
            text-shadow: 2px 2px 4px #000;
        }
        input, button {
            width: 100%;
            padding: 12px;
            margin: 12px 0;
            font-size: 16px;
            border-radius: 8px;
            border: none;
        }
        button {
            background: gold;
            color: #000;
            font-weight: bold;
            font-size: 18px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.5);
        }
        .slot {
            display: flex;
            justify-content: space-between;
            font-size: 60px;
            margin: 25px 0;
            background: #222;
            border-radius: 10px;
            padding: 10px;
        }
        .score, .ranking {
            margin-top: 20px;
            background: rgba(0,0,0,0.4);
            padding: 10px;
            border-radius: 8px;
        }
        .hidden { display: none; }
        ul { list-style: none; padding: 0; }
        li { font-size: 18px; padding: 4px 0; }
    </style>
</head>
<body>
    <div class="container" id="loginScreen">
        <h1>🎰 DaviSlots 🎰</h1>
        <input type="text" id="username" placeholder="Digite seu nome">
        <button onclick="login()">Entrar</button>
    </div>

    <div class="container hidden" id="gameScreen">
        <h2 id="welcome"></h2>
        <div class="slot">
            <div id="slot1">🍒</div>
            <div id="slot2">🍋</div>
            <div id="slot3">🍊</div>
        </div>
        <button onclick="girar()">Girar</button>
        <div class="score">Pontuação: <span id="pontos">0</span></div>

        <div class="ranking">
            <h3>Ranking</h3>
            <ul id="rankingList"></ul>
        </div>
    </div>

    <script>
        let pontos = 0;
        let jogador = "";
        let ranking = JSON.parse(localStorage.getItem("ranking")) || [];

        function login() {
            jogador = document.getElementById("username").value;
            if (jogador.trim() === "") return alert("Digite um nome!");
            document.getElementById("loginScreen").classList.add("hidden");
            document.getElementById("gameScreen").classList.remove("hidden");
            document.getElementById("welcome").textContent = "Bem-vindo ao DaviSlots, " + jogador + "!";
            atualizarRanking();
        }

        function girar() {
            const emojis = ["🍒", "🍋", "🍊", "⭐", "🔔", "7️⃣"];
            let s1 = emojis[Math.floor(Math.random() * emojis.length)];
            let s2 = emojis[Math.floor(Math.random() * emojis.length)];
            let s3 = emojis[Math.floor(Math.random() * emojis.length)];
            document.getElementById("slot1").textContent = s1;
            document.getElementById("slot2").textContent = s2;
            document.getElementById("slot3").textContent = s3;

            if (s1 === s2 && s2 === s3) {
                pontos += 50;
                alert("Jackpot! +50 pontos");
            } else if (s1 === s2 || s2 === s3 || s1 === s3) {
                pontos += 10;
                alert("Parzinho! +10 pontos");
            } else {
                pontos += 1;
            }
            document.getElementById("pontos").textContent = pontos;
            salvarRanking();
        }

        function salvarRanking() {
            let player = ranking.find(p => p.nome === jogador);
            if (player) {
                if (pontos > player.pontos) player.pontos = pontos;
            } else {
                ranking.push({ nome: jogador, pontos });
            }
            ranking.sort((a, b) => b.pontos - a.pontos);
            localStorage.setItem("ranking", JSON.stringify(ranking));
            atualizarRanking();
        }

        function atualizarRanking() {
            let list = document.getElementById("rankingList");
            list.innerHTML = "";
            ranking.forEach(p => {
                let li = document.createElement("li");
                li.textContent = `${p.nome}: ${p.pontos} pts`;
                list.appendChild(li);
            });
        }
    </script>
</body>
</html>
