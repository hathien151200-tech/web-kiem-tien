# web-kiem-tien
<!DOCTYPE html>
<html>
<head>
  <title>Kiếm tiền online</title>
  <style>
    body {
      margin: 0;
      font-family: Arial;
      background: #0f172a;
      color: white;
    }

    header {
      background: #020617;
      padding: 15px;
      text-align: center;
      font-size: 20px;
    }

    #money {
      color: lime;
      text-align: center;
      margin: 10px;
      font-size: 18px;
    }

    .container {
      padding: 15px;
    }

    .task {
      background: #1e293b;
      padding: 15px;
      border-radius: 10px;
      margin-bottom: 15px;
    }

    button {
      padding: 10px;
      border: none;
      border-radius: 8px;
      background: orange;
      margin-top: 10px;
    }

    input {
      padding: 10px;
      border-radius: 8px;
      border: none;
      width: 80%;
      margin-top: 10px;
    }
  </style>
</head>

<body>

<header>💰 Hệ thống kiếm tiền</header>

<div id="app"></div>

<script>
let money = localStorage.getItem("money") || 0;
let user = localStorage.getItem("user");

function saveMoney() {
  localStorage.setItem("money", money);
}

// ================= LOGIN =================
if (!user) {
  document.getElementById("app").innerHTML = `
    <div style="text-align:center;margin-top:50px;">
      <h2>Đăng nhập</h2>
      <input id="name" placeholder="Nhập tên"><br>
      <button onclick="login()">Vào hệ thống</button>
    </div>
  `;
}

function login() {
  let name = document.getElementById("name").value;
  if (!name) {
    alert("Nhập tên đi!");
    return;
  }
  localStorage.setItem("user", name);
  location.reload();
}

// ================= MAIN =================
if (user) {
  document.getElementById("app").innerHTML = `
    <div id="money">👤 ${user} | 💰 ${money}đ</div>
    <div class="container">

      <div class="task">
        <p>🔗 Nhiệm vụ 1 (+100đ)</p>
        <button onclick="startTask(100, 'https://google.com')">Làm</button>
      </div>

      <div class="task">
        <p>🔗 Nhiệm vụ 2 (+200đ)</p>
        <button onclick="startTask(200, 'https://youtube.com')">Làm</button>
      </div>

      <div id="taskBox"></div>

    </div>
  `;
}

// ================= TASK =================
function startTask(reward, link) {
  document.getElementById("taskBox").innerHTML = `
    <div class="task">
      <h3>🔒 Xác minh</h3>
      <input type="checkbox" id="check"> Tôi không phải robot
      <br>
      <button onclick="verify(${reward}, '${link}')">Tiếp tục</button>
    </div>
  `;
}

function verify(reward, link) {
  let check = document.getElementById("check");

  if (!check.checked) {
    alert("❌ Chưa xác minh!");
    return;
  }

  let time = 5;
  let box = document.getElementById("taskBox");

  let timer = setInterval(() => {
    box.innerHTML = "<p>⏳ Đang xử lý... " + time + "</p>";
    time--;

    if (time < 0) {
      clearInterval(timer);
      localStorage.setItem("reward", reward);
      window.location.href = link;
    }
  }, 1000);
}

// ================= RETURN =================
let reward = localStorage.getItem("reward");

if (reward) {
  money = parseInt(money) + parseInt(reward);
  saveMoney();
  localStorage.removeItem("reward");

  alert("✅ Hoàn thành nhiệm vụ +" + reward + "đ");
}
</script>

</body>
</html>
