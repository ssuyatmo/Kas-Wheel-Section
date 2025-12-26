
<!DOCTYPE html>
<html>
<head>
  <title>Aplikasi Kas</title>
  <style>
    body { font-family: Arial; padding: 20px; }
    input, select, button {
      padding: 8px;
      margin: 5px;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 15px;
    }
    table, th, td {
      border: 1px solid black;
    }
    th, td {
      padding: 8px;
      text-align: center;
    }
    .logout {
      background: red;
      color: white;
      border: none;
      padding: 8px;
      float: right;
    }
  </style>
</head>
<body>

<script>
  if (localStorage.getItem("login") !== "true") {
    window.location.href = "index.html";
  }
</script>

<button class="logout" onclick="logout()">Logout</button>
<h2>Aplikasi Kas</h2>

<h3>Saldo: Rp <span id="saldo">0</span></h3>

<input type="date" id="tanggal">
<input type="text" id="keterangan" placeholder="Keterangan">
<input type="number" id="jumlah" placeholder="Jumlah">
<select id="jenis">
  <option value="masuk">Pemasukan</option>
  <option value="keluar">Pengeluaran</option>
</select>
<button onclick="tambahKas()">Simpan</button>

<table>
  <thead>
    <tr>
      <th>Tanggal</th>
      <th>Keterangan</th>
      <th>Masuk</th>
      <th>Keluar</th>
    </tr>
  </thead>
  <tbody id="dataKas"></tbody>
</table>

<script>
  let kas = JSON.parse(localStorage.getItem("kas")) || [];

  function render() {
    let saldo = 0;
    let html = "";

    kas.forEach(k => {
      if (k.jenis === "masuk") saldo += k.jumlah;
      else saldo -= k.jumlah;

      html += `
        <tr>
          <td>${k.tanggal}</td>
          <td>${k.keterangan}</td>
          <td>${k.jenis === "masuk" ? k.jumlah : ""}</td>
          <td>${k.jenis === "keluar" ? k.jumlah : ""}</td>
        </tr>
      `;
    });

    document.getElementById("dataKas").innerHTML = html;
    document.getElementById("saldo").innerText = saldo.toLocaleString();
    localStorage.setItem("kas", JSON.stringify(kas));
  }

  function tambahKas() {
    kas.push({
      tanggal: tanggal.value,
      keterangan: keterangan.value,
      jumlah: parseInt(jumlah.value),
      jenis: jenis.value
    });
    render();
  }

  function logout() {
    localStorage.removeItem("login");
    window.location.href = "index.html";
  }

  render();
</script>

</body>
</html>
