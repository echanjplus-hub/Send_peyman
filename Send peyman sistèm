<!DOCTYPE html>
<html lang="ht">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Peman - Echanj Plus</title>
<style>
  body {
    font-family: Arial, sans-serif;
    background: #0a1f44;
    color: white;
    text-align: center;
    margin: 0;
    padding: 0;
  }
  header img {
    width: 140px;
    margin: 20px auto;
    display: block;
  }
  h1 { font-style: italic; color: #FFD700; }
  .balance-box {
    background: #112b60;
    margin: 15px auto;
    padding: 20px;
    border-radius: 12px;
    width: 85%;
    max-width: 420px;
    box-shadow: 0 0 10px rgba(0,0,0,0.3);
  }
  .btn {
    background: #FFD700;
    color: #0a1f44;
    border: none;
    padding: 12px 20px;
    border-radius: 8px;
    font-size: 16px;
    margin: 10px 0;
    cursor: pointer;
    width: 80%;
    max-width: 300px;
  }
  .btn:hover { background: #ffc107; }
  #withdrawSection {
    display: none;
    margin-top: 20px;
    text-align: left;
    background: #173b80;
    padding: 20px;
    border-radius: 12px;
    width: 90%;
    max-width: 440px;
    margin-left: auto;
    margin-right: auto;
  }
  #withdrawSection label {
    display: block;
    margin: 10px 0 5px;
  }
  #withdrawSection input, #withdrawSection select {
    width: 100%;
    padding: 10px;
    border-radius: 6px;
    border: none;
    margin-bottom: 15px;
  }
  footer { margin: 30px 0; }
  footer button {
    background: #fff;
    color: #0a1f44;
    border: none;
    padding: 10px 18px;
    border-radius: 8px;
    font-size: 15px;
    cursor: pointer;
  }
</style>
</head>
<body>

<header>
  <img src="https://i.postimg.cc/L8VgxQq9/image1-removebg-preview.png" alt="Logo Echanj Plus">
  <h1>Peman</h1>
</header>

<section class="balance-box">
  <h2>Sale Balance</h2>
  <p id="saleBalance">0.00 minit</p>
</section>

<section class="balance-box">
  <h2>Withdraw Balance</h2>
  <p id="withdrawBalance">0.00 Gdes</p>
  <button class="btn" onclick="showWithdraw()">Retre</button>
</section>

<section id="withdrawSection">
  <h3>Fè yon Retrè</h3>
  <label for="paymentMethod">Metòd Peman:</label>
  <select id="paymentMethod" onchange="toggleFields()">
    <option value="">-- Chwazi --</option>
    <option value="MonCash">Mon Cash</option>
    <option value="Natcash">Natcash</option>
    <option value="Paryaj Lakay">Paryaj Lakay</option>
    <option value="Paryaj Pam">Paryaj Pam</option>
    <option value="Biwo ARS">Biwo ARS</option>
  </select>

  <div id="nameField" style="display:none;">
    <label for="accountName">Non Kont:</label>
    <input type="text" id="accountName" placeholder="Antre non kont">
  </div>

  <div id="numberField" style="display:none;">
    <label for="accountNumber">Nimewo Kont:</label>
    <input type="text" id="accountNumber" placeholder="Antre nimewo kont">
  </div>

  <div id="amountField" style="display:none;">
    <label for="withdrawAmount">Kantite Retrè:</label>
    <input type="number" id="withdrawAmount" placeholder="Antre kantite kob">
  </div>

  <p id="biwoInfo" style="display:none; font-size:14px; color:#FFD700;">
    📍 Ale nan: Wout Nasyonal 72, Kafou Waf Ansdeno 🚩<br>
    ⚠️ Kopi kòd tranzaksyon ou pou resevwa lajan.
  </p>

  <button class="btn" onclick="sendPayment()">Send Payment</button>
  <p id="paymentFeedback"></p>
</section>

<footer>
  <button onclick="goHome()">← Retounen Home</button>
</footer>

<script>
let saleBalance = 0.00;
let withdrawBalance = 0.00;
let transactionId = null;
const minimumBalance = 1.0; // Minimòm obligatwa sou kont apre retrè

function updateBalances() {
  document.getElementById("saleBalance").innerText = saleBalance.toFixed(2) + " minit";
  document.getElementById("withdrawBalance").innerText = withdrawBalance.toFixed(2) + " Gdes";
}

function showWithdraw() {
  document.getElementById("withdrawSection").style.display = "block";
}

function toggleFields() {
  const method = document.getElementById("paymentMethod").value;
  document.getElementById("nameField").style.display = (method === "MonCash" || method === "Natcash") ? "block" : "none";
  document.getElementById("numberField").style.display = (method !== "Biwo ARS" && method !== "") ? "block" : "none";
  document.getElementById("amountField").style.display = method !== "" ? "block" : "none";
  document.getElementById("biwoInfo").style.display = (method === "Biwo ARS") ? "block" : "none";
}

// Lè transfè soti nan Home fèt
function setBalancesFromFirebase(data) {
  saleBalance = data.saleBalance || 0;
  const fee = saleBalance * 0.145;
  withdrawBalance = saleBalance - fee;
  transactionId = data.transactionId || "ARS-" + Math.floor(Math.random()*10000);
  updateBalances();
}

function sendPayment() {
  const method = document.getElementById("paymentMethod").value;
  const name = document.getElementById("accountName").value;
  const number = document.getElementById("accountNumber").value;
  const amount = parseFloat(document.getElementById("withdrawAmount").value);

  if (withdrawBalance <= minimumBalance || !transactionId) {
    document.getElementById("paymentFeedback").innerText = "❌ Ou pa gen balans ase pou retrè.";
    return;
  }

  if (!method || isNaN(amount) || amount <= 0 || amount > withdrawBalance - minimumBalance) {
    document.getElementById("paymentFeedback").innerText = `❌ Kantite retrè pa valab. Ou dwe kite omwen ${minimumBalance} Gdes sou kont lan.`;
    return;
  }

  if ((method === "MonCash" || method === "Natcash") && !name) {
    document.getElementById("paymentFeedback").innerText = "❌ Non kont obligatwa pou metòd sa.";
    return;
  }

  if ((method !== "Biwo ARS") && !number) {
    document.getElementById("paymentFeedback").innerText = "❌ Nimewo kont obligatwa.";
    return;
  }

  const fee = amount * 0.145;
  const net = amount - fee;

  // Desann withdraw balance apre retrè
  withdrawBalance -= amount;
  updateBalances();

  let msg = `🔔 DEMAND RETRÈ\n\n` +
            `🔑 Transaction ID: ${transactionId}\n` +
            `💳 Metòd: ${method}\n` +
            (name ? `👤 Non kont: ${name}\n` : "") +
            (number ? `📱 Nimewo: ${number}\n` : "") +
            `💰 Kantite: ${amount.toFixed(2)} HTG\n` +
            `💸 Frè Sistèm (14.5%): ${fee.toFixed(2)} HTG\n` +
            `✅ Net: ${net.toFixed(2)} HTG\n`;

  const whatsappUrl = `https://wa.me/50947111123?text=${encodeURIComponent(msg)}`;
  window.open(whatsappUrl, "_blank");
}

function goHome() {
  window.location.href = "home.html"; // Ranplase ak chemen reyèl home
}

// Inisyal balans 0
updateBalances();
</script>

</body>
</html>
