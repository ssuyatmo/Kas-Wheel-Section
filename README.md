<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<title>Aplikasi Kas</title>
<style>
body { font-family: Arial; background:#f4f4f4; }
.container { width:90%; max-width:900px; margin:auto; background:#fff; padding:15px; margin-top:30px; border-radius:5px; }
h2 { text-align:center; }
input, select, button { padding:6px; margin:5px 0; width:100%; }
table { width:100%; border-collapse:collapse; margin-top:10px; }
table, th, td { border:1px solid #ccc; }
th, td { padding:6px; text-align:center; }
.hidden { display:none; }
.logout { background:red; color:white; border:none; }
</style>
</head>
<body>

<!-- LOGIN -->
<div class="container" id="loginPage">
<h2>Login Kas</h2>
<input type="text" id="username" placeholder="Username">
<input type="password" id="password" placeholder="Password">
<button onclick="login()">Login</button>
<p id="loginMsg" style="color:red;"></p>
</div>

<!-- APLIKASI KAS -->
<div class="container hidden" id="appPage">
<h2>Aplikasi Kas</h2>

<button class="logout" onclick="logout()">Logout</button>

<h3>Input Kas</h3>
<input type="text" id="nama" placeholder="Nama">
<input type="month" id="bulan">
<select id="jenis">
<option value="masuk">Pemasukan</option>
<option value="keluar">Pengeluaran</option>
</select>
<input type="text" id="ket" placeholder="Keterangan">
<input type="number" id="nominal" placeholder="Nominal">
<button onclick="tambahKas()">Simpan</button>

<h3>Data Kas</h3>
<table>
<thead>
<tr>
<th>No</th>
<th>Nama</th>
<th>Bulan</th>
<th>Jenis</th>
<th>Keterangan</th>
<th>Nominal</th>
</tr>
</thead>
<tbody id="tabelKas"></tbody>
</table>

<h3>Saldo: Rp <span id="saldo">0</span></h3>
</div>

<script>
let dataKas = JSON.parse(localStorage.getItem("kas")) || [];

// LOGIN
function login(){
  let u = document.getElementById("username").value;
  let p = document.getElementById("password").value;

  if(u==="admin" && p==="12345"){
    localStorage.setItem("login","true");
    tampilApp();
  }else{
    document.getElementById("loginMsg").innerText="Username / Password salah";
  }
}

// LOGOUT
function logout(){
  localStorage.removeItem("login");
  location.reload();
}

// TAMPIL APLIKASI
function tampilApp(){
  document.getElementById("loginPage").classList.add("hidden");
  document.getElementById("appPage").classList.remove("hidden");
  tampilData();
}

// TAMBAH KAS
function tambahKas(){
  let kas = {
    nama: nama.value,
    bulan: bulan.value,
    jenis: jenis.value,
    ket: ket.value,
    nominal: parseInt(nominal.value)
  };
  dataKas.push(kas);
  localStorage.setItem("kas", JSON.stringify(dataKas));
  tampilData();
}

// TAMPIL DATA
function tampilData(){
  let tbody = "";
  let saldo = 0;
  dataKas.forEach((k,i)=>{
    if(k.jenis==="masuk") saldo += k.nominal;
    else saldo -= k.nominal;

    tbody += `<tr>
      <td>${i+1}</td>
      <td>${k.nama}</td>
      <td>${k.bulan}</td>
      <td>${k.jenis}</td>
      <td>${k.ket}</td>
      <td>Rp ${k.nominal.toLocaleString()}</td>
    </tr>`;
  });
  document.getElementById("tabelKas").innerHTML = tbody;
  document.getElementById("saldo").innerText = saldo.toLocaleString();
}

// AUTO LOGIN
if(localStorage.getItem("login")==="true"){
  tampilApp();
}
</script>

</body>
</html>
