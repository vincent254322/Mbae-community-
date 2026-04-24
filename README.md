<!DOCTYPE html>
<html>
<head>
  <title>Mbae Community</title>

  <style>
    body {
      font-family: Arial;
      background: #f4f4f4;
      margin: 0;
      text-align: center;
    }

    header {
      background: green;
      color: white;
      padding: 15px;
    }

    .container {
      padding: 15px;
    }

    .box {
      background: white;
      padding: 15px;
      margin: 15px auto;
      width: 320px;
      border-radius: 10px;
    }

    input {
      width: 90%;
      padding: 10px;
      margin: 5px;
    }

    button {
      padding: 10px;
      background: green;
      color: white;
      border: none;
      width: 95%;
      margin-top: 5px;
    }

    .hidden {
      display: none;
    }

    table {
      width: 100%;
      background: white;
      border-collapse: collapse;
    }

    td, th {
      border: 1px solid #ddd;
      padding: 8px;
    }
  </style>

</head>
<body>

<header>
  <h2>Mbae Community ❤️</h2>
  <p>🚧 DEMO MODE (No real money)</p>
</header>

<div class="container">

  <!-- USER SECTION -->
  <div id="userSection">

    <div class="box">
      <h3>Help Brian go to school</h3>
      <input id="name" placeholder="Your Name">
      <input id="phone" placeholder="Phone (2547XXXXXXXX)">
      <input id="amount" placeholder="Amount">
      <button onclick="donate()">Donate</button>
    </div>

    <div class="box">
      <h3>Total Raised: KSh <span id="total">0</span></h3>
    </div>

    <div class="box">
      <h3>Recent Donations</h3>
      <ul id="list"></ul>
    </div>

    <button onclick="showAdmin()">Admin</button>
  </div>

  <!-- ADMIN LOGIN -->
  <div id="loginSection" class="hidden">
    <div class="box">
      <h3>Admin Login</h3>
      <input id="pass" type="password" placeholder="Password">
      <button onclick="login()">Login</button>
      <button onclick="goHome()">Back</button>
    </div>
  </div>

  <!-- ADMIN DASHBOARD -->
  <div id="adminSection" class="hidden">
    <h3>Admin Dashboard</h3>

    <table>
      <tr>
        <th>Name</th>
        <th>Phone</th>
        <th>Amount</th>
        <th>Code</th>
      </tr>
      <tbody id="adminData"></tbody>
    </table>

    <button onclick="clearAll()">Clear All</button>
    <button onclick="logout()">Logout</button>
  </div>

</div>

<script>
const ADMIN_PASSWORD = "1234";

/* STORAGE */
function getDonations() {
  return JSON.parse(localStorage.getItem("donations") || "[]");
}

function saveDonations(data) {
  localStorage.setItem("donations", JSON.stringify(data));
}

/* PHONE VALIDATION */
function validPhone(phone) {
  return /^2547\\d{8}$/.test(phone);
}

/* DONATE */
function donate() {
  const name = document.getElementById("name").value;
  const phone = document.getElementById("phone").value;
  const amount = document.getElementById("amount").value;

  if (!name || !phone || !amount) {
    alert("Fill all fields");
    return;
  }

  if (!validPhone(phone)) {
    alert("Enter valid phone (2547XXXXXXXX)");
    return;
  }

  alert("📲 Sending M-Pesa request...");

  setTimeout(() => {
    let donations = getDonations();

    donations.push({
      name,
      phone,
      amount: Number(amount),
      code: "DEMO" + Math.floor(Math.random() * 1000000)
    });

    saveDonations(donations);

    alert("✅ Payment Successful (Demo)");
    load();
  }, 1500);
}

/* LOAD USER VIEW */
function load() {
  const donations = getDonations();

  let total = 0;
  const list = document.getElementById("list");
  list.innerHTML = "";

  donations.slice().reverse().forEach(d => {
    total += d.amount;
    list.innerHTML += `<li>${d.name} donated KSh ${d.amount}</li>`;
  });

  document.getElementById("total").innerText = total;
}

/* ADMIN */
function showAdmin() {
  document.getElementById("userSection").classList.add("hidden");
  document.getElementById("loginSection").classList.remove("hidden");
}

function login() {
  const pass = document.getElementById("pass").value;

  if (pass === ADMIN_PASSWORD) {
    document.getElementById("loginSection").classList.add("hidden");
    document.getElementById("adminSection").classList.remove("hidden");
    loadAdmin();
  } else {
    alert("Wrong password");
  }
}

function loadAdmin() {
  const data = getDonations();
  const table = document.getElementById("adminData");

  table.innerHTML = "";

  data.slice().reverse().forEach(d => {
    table.innerHTML += `
      <tr>
        <td>${d.name}</td>
        <td>${d.phone}</td>
        <td>${d.amount}</td>
        <td>${d.code}</td>
      </tr>
    `;
  });
}

function clearAll() {
  if (confirm("Delete all donations?")) {
    localStorage.removeItem("donations");
    loadAdmin();
    load();
  }
}

function logout() {
  document.getElementById("adminSection").classList.add("hidden");
  document.getElementById("userSection").classList.remove("hidden");
}

function goHome() {
  document.getElementById("loginSection").classList.add("hidden");
  document.getElementById("userSection").classList.remove("hidden");
}

/* INIT */
load();
</script>

</body>
</html>
