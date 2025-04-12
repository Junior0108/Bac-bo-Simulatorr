# Bac-bo-Simulatorr<!DOCTYPE html><html lang="pt">
<head>
  <meta charset="UTF-8">
  <title>Simulador de Bac Bo</title>
  <style>
    body { font-family: sans-serif; background: #111; color: #eee; text-align: center; padding: 20px; }
    button { margin: 10px; padding: 10px 20px; font-size: 16px; cursor: pointer; }
    .resultados, .historico { margin-top: 20px; }
    .dados { font-size: 24px; }
  </style>
</head>
<body>
  <h1>Simulador de Bac Bo - Estratégia Paroli</h1>
  <p>Saldo: <span id="saldo">1000</span> Kz</p>
  <p>Aposta atual: <span id="aposta">100</span> Kz</p>
  <div>
    <button onclick="apostar('banker')">Apostar no Banker</button>
    <button onclick="apostar('player')">Apostar no Player</button>
    <button onclick="apostar('tie')">Apostar no Tie</button>
  </div>
  <div class="resultados">
    <p class="dados" id="dados">--</p>
    <p id="vencedor">Aguardando aposta...</p>
  </div>
  <div class="historico">
    <h3>Histórico</h3>
    <ul id="historico"></ul>
  </div>  <script>
    let saldo = 1000;
    let apostaBase = 100;
    let apostaAtual = apostaBase;
    let vitoriasSeguidas = 0;

    function rolarDado() {
      return Math.floor(Math.random() * 6) + 1;
    }

    function apostar(tipo) {
      if (saldo < apostaAtual) {
        alert("Saldo insuficiente!");
        return;
      }

      let p1 = rolarDado(), p2 = rolarDado();
      let b1 = rolarDado(), b2 = rolarDado();
      let playerTotal = p1 + p2;
      let bankerTotal = b1 + b2;

      let vencedor = 'empate';
      if (playerTotal > bankerTotal) vencedor = 'player';
      else if (bankerTotal > playerTotal) vencedor = 'banker';

      let ganhou = tipo === vencedor;

      if (tipo === 'tie' && vencedor === 'empate') {
        saldo += apostaAtual * 8;
        ganhou = true;
      } else if (ganhou) {
        saldo += apostaAtual;
      } else {
        saldo -= apostaAtual;
      }

      document.getElementById("dados").textContent = `Player: ${p1}+${p2} = ${playerTotal} | Banker: ${b1}+${b2} = ${bankerTotal}`;
      document.getElementById("vencedor").textContent = `Vencedor: ${vencedor.toUpperCase()}${ganhou ? " (Tu ganhaste!)" : " (Perdeste)"}`;

      // Estratégia Paroli
      if (ganhou) {
        vitoriasSeguidas++;
        apostaAtual = apostaBase * Math.pow(2, vitoriasSeguidas);
        if (vitoriasSeguidas >= 2) {
          apostaAtual = apostaBase;
          vitoriasSeguidas = 0;
        }
      } else {
        apostaAtual = apostaBase;
        vitoriasSeguidas = 0;
      }

      atualizar();
      adicionarHistorico(tipo, playerTotal, bankerTotal, vencedor, ganhou);
    }

    function atualizar() {
      document.getElementById("saldo").textContent = saldo;
      document.getElementById("aposta").textContent = apostaAtual;
    }

    function adicionarHistorico(tipo, pt, bt, v, g) {
      const item = document.createElement("li");
      item.textContent = `Apostaste em ${tipo.toUpperCase()} | Player: ${pt} | Banker: ${bt} | Vencedor: ${v.toUpperCase()} | ${g ? "Ganhou" : "Perdeu"}`;
      document.getElementById("historico").prepend(item);
    }
  </script></body>
</html>
