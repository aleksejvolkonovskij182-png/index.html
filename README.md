# index.html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Evolution Tycoon</title>
    <style>
        body { font-family: 'Segoe UI', sans-serif; background: #1a1a1a; color: white; margin: 0; padding: 20px; display: flex; flex-direction: column; align-items: center; }
        .balance-box { background: #2d2d2d; padding: 20px; border-radius: 20px; width: 90%; text-align: center; border-bottom: 5px solid #4CAF50; margin-bottom: 20px; }
        #balance { font-size: 32px; color: #4CAF50; margin: 5px 0; }
        .click-btn { width: 150px; height: 150px; border-radius: 50%; border: none; background: radial-gradient(#4CAF50, #2e7d32); color: white; font-weight: bold; font-size: 20px; cursor: pointer; box-shadow: 0 10px #1b5e20; active: transform: translateY(4px); active: box-shadow: 0 5px #1b5e20; }
        .upgrades-list { width: 100%; margin-top: 20px; }
        .upgrade-card { background: #333; padding: 15px; border-radius: 12px; margin-bottom: 10px; display: flex; justify-content: space-between; align-items: center; }
        .upgrade-card button { background: #444; border: 1px solid #4CAF50; color: white; padding: 8px; border-radius: 5px; }
        .evo-btn { background: linear-gradient(45deg, #ff9800, #f44336); width: 100%; padding: 15px; border-radius: 12px; border: none; color: white; font-weight: bold; margin-top: 20px; display: none; }
    </style>
</head>
<body>

    <div class="balance-box">
        <div id="era-label" style="color: #ffa726; font-weight: bold;">ЭРА: ДИНОЗАВРЫ</div>
        <div id="balance">0</div>
        <div id="currency-label">Костей</div>
        <div style="font-size: 12px; opacity: 0.7;">Доход в сек: <span id="pps">0</span></div>
    </div>

    <button class="click-btn" onclick="handleTap()">ТАПАЙ!</button>

    <div class="upgrades-list" id="upgrades">
        </div>

    <button id="evo-btn" class="evo-btn" onclick="evolve()">КУПИТЬ НОВУЮ ЖИЗНЬ (ЭВОЛЮЦИЯ)</button>

<script>
    let balance = 0;
    let pps = 0;
    let currentEra = 0;

    const eras = [
        { name: "ДИНОЗАВРЫ", curr: "Костей", goal: 1000, color: "#4CAF50", upgrades: [{n: "Украсть яйцо", p: 15, i: 1}, {n: "Пещерный уют", p: 100, i: 8}] },
        { name: "ДРЕВНИЙ МИР", curr: "Золота", goal: 5000, color: "#ffeb3b", upgrades: [{n: "Глиняная хижина", p: 20, i: 2}, {n: "Кузница", p: 150, i: 12}] },
        { name: "БУДУЩЕЕ", curr: "Квантов", goal: 999999, color: "#00bcd4", upgrades: [{n: "Нейросеть", p: 100, i: 20}, {n: "Звезда Смерти", p: 1000, i: 150}] }
    ];

    function updateUI() {
        const era = eras[currentEra];
        document.getElementById('balance').innerText = Math.floor(balance);
        document.getElementById('era-label').innerText = "ЭРА: " + era.name;
        document.getElementById('era-label').style.color = era.color;
        document.getElementById('currency-label').innerText = era.curr;
        document.getElementById('pps').innerText = pps.toFixed(1);
        
        // Показ кнопки эволюции
        if (balance >= era.goal && currentEra < eras.length - 1) {
            document.getElementById('evo-btn').style.display = 'block';
            document.getElementById('evo-btn').innerText = `ЭВОЛЮЦИЯ В ${eras[currentEra+1].name} (Цена: ${era.goal})`;
        } else {
            document.getElementById('evo-btn').style.display = 'none';
        }
    }

    function handleTap() {
        balance += 1 + (currentEra * 5);
        updateUI();
    }

    function buyUpgrade(index) {
        const upg = eras[currentEra].upgrades[index];
        if (balance >= upg.p) {
            balance -= upg.p;
            pps += upg.i;
            upg.p = Math.floor(upg.p * 1.5);
            renderUpgrades();
            updateUI();
        }
    }

    function evolve() {
        if (balance >= eras[currentEra].goal) {
            balance = 0;
            pps = 0;
            currentEra++;
            renderUpgrades();
            updateUI();
            alert("Поздравляем! Вы переродились в новой эре!");
        }
    }

    function renderUpgrades() {
        const container = document.getElementById('upgrades');
        container.innerHTML = '';
        eras[currentEra].upgrades.forEach((u, i) => {
            container.innerHTML += `
                <div class="upgrade-card">
                    <div><b>${u.n}</b><br><small>Доход: +${u.i}/сек</small></div>
                    <button onclick="buyUpgrade(${i})">Купить: ${u.p}</button>
                </div>`;
        });
    }

    setInterval(() => {
        balance += pps / 10;
        updateUI();
    }, 100);

    renderUpgrades();
</script>
</body>
</html>
