<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <title>🔐 Quà tặng bí mật</title>
  <style>
    body {
      margin: 0;
      font-family: 'Arial', sans-serif;
      background-color: #000;
      color: white;
      overflow: hidden;
    }
    .center-box {
      position: absolute;
      top: 40%;
      width: 100%;
      text-align: center;
      z-index: 10;
    }
    input, button {
      padding: 10px 20px;
      font-size: 1.1em;
      border-radius: 5px;
      border: none;
      margin-top: 10px;
    }
    .chuc {
      position: absolute;
      top: 40%;
      width: 100%;
      text-align: center;
      font-size: 1.6em;
      color: #fff;
      animation: fadeIn 2s ease-in-out;
      z-index: 100;
    }
    @keyframes fadeIn {
      0% {opacity: 0;}
      100% {opacity: 1;}
    }
    canvas {
      display: block;
      position: fixed;
      top: 0; left: 0;
      width: 100vw;
      height: 100vh;
      z-index: 50;
    }
  </style>
</head>
<body>

<canvas id="canvas" style="display:none;"></canvas>

<div class="center-box" id="unlockBox">
  <h2>🔒 Nhập mật mã để mở phần thưởng</h2>
  <input type="password" id="password" placeholder="Mật mã bí mật..." />
  <br />
  <button onclick="kiemTra()">MỞ QUÀ</button>
  <p id="thongBao" style="color: red;"></p>
</div>

<div class="chuc" id="loiChuc" style="display:none;"></div>

<script>
  const MAT_KHAU_DUNG = "dothithuyuyen";  // Mật khẩu bạn đã cung cấp
  const loiChucList = [
    "🍜 Chúc mày ăn mì cay cấp độ 7 mà không rơi một giọt lệ!",
    "🍢 Xiên bẩn là đỉnh cao, cũng như tình bạn tụi mình!",
    "🍋 Một ly trà chanh, một tình bạn chanh sả kéo dài mãi mãi!",
    "🔥 Tình bạn của tụi mình nóng như nồi lẩu xiên cay nồng!",
    "🎉 Mật mã đúng rồi! Đây là phần thưởng dành cho bạn thân cực phẩm!"
  ];

  function kiemTra() {
    const pass = document.getElementById("password").value.trim();
    if (pass === MAT_KHAU_DUNG) {
      document.getElementById("unlockBox").style.display = "none";
      document.getElementById("canvas").style.display = "block";
      const loiChuc = document.getElementById("loiChuc");
      loiChuc.style.display = "block";
      loiChuc.innerText = loiChucList[Math.floor(Math.random() * loiChucList.length)];
      startFireworks();
    } else {
      document.getElementById("thongBao").innerText = "Sai mật mã rồi bạn iuuu 😢";
    }
  }

  // Pháo hoa
  const canvas = document.getElementById("canvas");
  const ctx = canvas.getContext("2d");
  let width, height;
  let particles = [];

  function resize() {
    width = window.innerWidth;
    height = window.innerHeight;
    canvas.width = width;
    canvas.height = height;
  }

  window.addEventListener("resize", resize);
  resize();

  class Particle {
    constructor(x, y, color, vx, vy) {
      this.x = x;
      this.y = y;
      this.color = color;
      this.vx = vx;
      this.vy = vy;
      this.alpha = 1;
      this.size = Math.random() * 3 + 2;
    }
    update() {
      this.x += this.vx;
      this.y += this.vy;
      this.alpha -= 0.015;
      this.vy += 0.02; // gravity effect
    }
    draw() {
      ctx.globalAlpha = this.alpha > 0 ? this.alpha : 0;
      ctx.fillStyle = this.color;
      ctx.beginPath();
      ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
      ctx.fill();
      ctx.globalAlpha = 1;
    }
  }

  function firework() {
    const x = Math.random() * width;
    const y = Math.random() * height / 2;
    const colors = ["#f44336", "#4caf50", "#2196f3", "#ffeb3b", "#9c27b0", "#00bcd4", "#ffffff"];

    for (let i = 0; i < 50; i++) {
      const angle = Math.random() * 2 * Math.PI;
      const speed = Math.random() * 5 + 1;
      const vx = Math.cos(angle) * speed;
      const vy = Math.sin(angle) * speed;
      const color = colors[Math.floor(Math.random() * colors.length)];
      particles.push(new Particle(x, y, color, vx, vy));
    }
  }

  function animate() {
    ctx.fillStyle = "rgba(0, 0, 0, 0.2)";
    ctx.fillRect(0, 0, width, height);

    particles = particles.filter(p => p.alpha > 0);
    particles.forEach(p => {
      p.update();
      p.draw();
    });

    requestAnimationFrame(animate);
  }

  let fireworkInterval;

  function startFireworks() {
    if (fireworkInterval) clearInterval(fireworkInterval);
    fireworkInterval = setInterval(firework, 800);
    animate();
  }
</script>

</body>
</html>
