# Box-Opener
<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Boxenchaos - Spiel</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            text-align: center;
            background-color: #f4f4f9;
        }
        h1 {
            color: #333;
        }
        .container {
            max-width: 600px;
            margin: 20px auto;
            padding: 20px;
            background: #ffffff;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            border-radius: 8px;
        }
        button {
            font-size: 18px;
            padding: 10px 20px;
            margin: 10px;
            border: none;
            border-radius: 5px;
            background-color: #007BFF;
            color: white;
            cursor: pointer;
        }
        button:hover:not(:disabled) {
            background-color: #0056b3;
        }
        .info {
            font-size: 16px;
            margin: 10px 0;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Boxenchaos - Spiel</h1>
        <p class="info">Münzen: <span id="coins">0</span></p>
        <button id="normalBox">Normale Box (2 Münzen)</button>
        <button id="rareBox">Seltene Box (5 Münzen)</button>
        <button id="goToShop">Zum Shop</button>
        <button id="freeBox">Gratis Box</button>
        <p class="info" id="result">Willkommen zu Boxenchaos!</p>
    </div>

    <script>
        let coins = parseInt(localStorage.getItem("coins")) || 10;
        const coinsElement = document.getElementById("coins");
        const resultElement = document.getElementById("result");

        let powerUpActive = false;

        const updateCoins = (amount) => {
            coins += amount;
            if (coins < 0) coins = 0;
            coinsElement.textContent = coins;
            localStorage.setItem("coins", coins); // Münzstand speichern
        };

        const activatePowerUp = () => {
            powerUpActive = true;
            resultElement.textContent = "Power-up aktiviert! Die nächste Belohnung wird verdoppelt.";
        };

        const openBox = (cost, boxType) => {
            if (coins >= cost) {
                updateCoins(-cost);
                let outcome = "";
                if (boxType === "normal") {
                    const outcomes = [
                        "+1 Münze",
                        "+3 Münzen",
                        "+5 Münzen",
                        "Nichts",
                        "Schlüssel",
                        "Power-up"
                    ];
                    outcome = outcomes[Math.floor(Math.random() * outcomes.length)];
                } else if (boxType === "rare") {
                    const outcomes = [
                        "+5 Münzen",
                        "+10 Münzen",
                        "Schlüssel",
                        "Nichts",
                        "+3 Münzen",
                        "Power-up"
                    ];
                    outcome = outcomes[Math.floor(Math.random() * outcomes.length)];
                } else if (boxType === "free") {
                    const outcomes = [
                        "+1 Münze",
                        "+3 Münzen",
                        "+5 Münzen"
                    ];
                    outcome = outcomes[0]; // Gratis Box hat nur einen Wert
                }

                if (outcome === "Power-up") {
                    activatePowerUp();
                } else {
                    resultElement.textContent = `Box-Inhalt: ${outcome}`;
                    if (outcome === "+1 Münze") updateCoins(1);
                    if (outcome === "+3 Münzen") updateCoins(3);
                    if (outcome === "+5 Münzen") updateCoins(5);
                    if (outcome === "+10 Münzen") updateCoins(10);
                    if (outcome === "Schlüssel") resultElement.textContent = "Du hast einen Schlüssel erhalten!";
                }

                if (powerUpActive) {
                    // Verdoppelt die Münzenbelohnung, wenn das Power-up aktiv ist
                    if (outcome.includes("Münzen")) {
                        let amount = parseInt(outcome.split(" ")[0]);
                        updateCoins(amount); // Verdoppelt die Münzen
                        resultElement.textContent = `Power-up aktiv! Du hast ${amount * 2} Münzen erhalten!`;
                        powerUpActive = false; // Power-up wird nach der Nutzung deaktiviert
                    }
                }
            } else {
                resultElement.textContent = "Nicht genug Münzen!";
            }
        };

        document.getElementById("normalBox").addEventListener("click", () => {
            openBox(2, "normal");
        });

        document.getElementById("rareBox").addEventListener("click", () => {
            openBox(5, "rare");
        });

        document.getElementById("freeBox").addEventListener("click", () => {
            openBox(0, "free");
        });

        document.getElementById("goToShop").addEventListener("click", () => {
            window.location.href = "shop.html";
        });

        // Zeige den aktuellen Münzstand an
        coinsElement.textContent = coins;
    </script>
</body>
</html>
<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Shop</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            text-align: center;
            background-color: #ffffff;
        }
        h1 {
            color: #333;
            margin: 20px 0;
        }
        .container {
            max-width: 600px;
            margin: 20px auto;
            padding: 20px;
            background: #ffffff;
        }
        button {
            font-size: 18px;
            padding: 

