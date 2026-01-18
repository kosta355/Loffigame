# loffigame
<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8" />
<title>LoffiGift</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>

<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Comfortaa:wght@400;500;700&family=Orbitron:wght@500;700&display=swap" rel="stylesheet">

<style>
  * {
    margin:0;
    padding:0;
    box-sizing:border-box;
  }

  body {
    min-height:100vh;
    font-family:'Comfortaa', system-ui, sans-serif;
    background: #0a0818;
    color:#e0e0ff;
    overflow-x:hidden;
    position:relative;
  }

  /* Падающие звёзды сверху вниз */
  .bg-star {
    position: fixed;
    top: -10vh;
    color: rgba(220, 220, 255, 0.18);
    font-size: clamp(12px, 2.4vw, 28px);
    pointer-events: none;
    animation: bgFall linear infinite;
  }

  @keyframes bgFall {
    0%   { transform: translateY(0vh) rotate(0deg); opacity: 0.35; }
    100% { transform: translateY(130vh) rotate(720deg); opacity: 0.02; }
  }

  #loading {
    position:fixed;
    inset:0;
    background:#08051a;
    display:flex;
    flex-direction:column;
    justify-content:center;
    align-items:center;
    z-index:9999;
    transition:opacity .8s;
  }

  #score-container {
    position: fixed;
    top: 16px;
    right: 20px;
    z-index: 100;
    font-family: 'Orbitron', monospace;
    font-size: 1.9rem;
    padding: 0.5rem 1.3rem;
    border-radius: 16px;
    background: rgba(25,28,65,0.65);
    backdrop-filter: blur(10px);
    border: 1px solid rgba(180,180,255,0.25);
    box-shadow: 0 5px 20px rgba(0,0,0,0.65);
    color: #e8e8ff;
  }

  .screen {
    min-height:100vh;
    padding: 2rem 1rem;
    display:none;
    flex-direction:column;
    align-items:center;
    justify-content:center;
  }
  .screen.active { display:flex; }

  .balance {
    font-family:'Orbitron', monospace;
    font-size:clamp(2rem,5.5vw,3.2rem);
    padding:0.8rem 2rem;
    border-radius:20px;
    background:rgba(20,22,60,0.5);
    backdrop-filter:blur(12px);
    border:1px solid rgba(180,180,255,0.22);
    box-shadow:0 8px 35px rgba(0,0,0,0.6);
    color:#e8e8ff;
    margin-bottom:2.2rem;
  }

  .gift {
    font-size:clamp(10rem,28vw,16rem);
    cursor:pointer;
    filter:drop-shadow(0 0 35px rgba(200,200,255,0.45));
    animation:gentle-float 8s ease-in-out infinite;
  }

  .gift.jump {
    animation:jump 0.5s;
  }

  .gift:hover {
    transform:scale(1.08);
    filter:drop-shadow(0 0 60px rgba(200,200,255,0.7));
  }

  @keyframes gentle-float {
    0%,100% { transform:translateY(0); }
    50% { transform:translateY(-25px); }
  }

  @keyframes jump {
    0%,100% { transform:translateY(0); }
    50% { transform:translateY(-70px); }
  }

  .roulette-container {
    width:90%;
    max-width:900px;
    height:200px;
    background:rgba(12,14,45,0.7);
    backdrop-filter:blur(16px);
    border-radius:28px;
    border:1px solid rgba(160,160,255,0.18);
    box-shadow:0 20px 70px rgba(0,0,0,0.7);
    overflow:hidden;
    position:relative;
    margin:2.4rem 0;
  }

  .roulette-pointer {
    position:absolute;
    top:-22px;
    left:50%;
    transform:translateX(-50%);
    width:0; height:0;
    border-left:30px solid transparent;
    border-right:30px solid transparent;
    border-bottom:45px solid #aa4444;
    filter:drop-shadow(0 0 12px #aa4444aa);
    z-index:10;
  }

  .roulette-strip {
    display:flex;
    height:100%;
  }

  .roulette-item {
    flex:0 0 170px;
    height:170px;
    margin:15px 8px;
    font-size:80px;
    line-height:170px;
    text-align:center;
    border-radius:20px;
    background:linear-gradient(145deg, #e0c040, #d08020);
    box-shadow:0 0 25px rgba(255,255,200,0.35);
    color:#111;
    font-weight:bold;
  }

  .roulette-strip.auto-spin {
    animation: fastRoulette 3.8s linear infinite;
  }

  @keyframes fastRoulette {
    from { transform: translateX(0); }
    to   { transform: translateX(-50%); }
  }

  button {
    font-size:1.55rem;
    font-weight:600;
    padding:1rem 3rem;
    border:none;
    border-radius:50px;
    background:linear-gradient(45deg, #d0a030, #b07010);
    color:#0f0c29;
    cursor:pointer;
    box-shadow:0 8px 30px rgba(180,140,0,0.45);
    transition: all 0.35s;
  }

  button:hover:not(:disabled) {
    transform:translateY(-5px) scale(1.06);
    box-shadow:0 16px 50px rgba(180,140,0,0.65);
  }

  button:disabled {
    opacity:0.45;
    cursor:not-allowed;
  }

  .nav {
    position:fixed;
    bottom:0;
    left:0; right:0;
    background:rgba(8,8,30,0.9);
    backdrop-filter:blur(16px);
    border-top:1px solid rgba(140,140,255,0.15);
    padding:0.8rem 1rem;
    display:flex;
    justify-content:space-around;
    z-index:100;
  }

  .nav button {
    padding:0.6rem 1.3rem;
    font-size:0.96rem;
    background:rgba(140,140,255,0.12);
    color:#d0d0ff;
    border:1px solid rgba(140,140,255,0.25);
    border-radius:12px;
    transition: all 0.3s;
  }

  .nav button.active,
  .nav button:hover {
    background:linear-gradient(45deg, #d0a030, #b07010);
    color:#0f0c29;
    transform:translateY(-4px);
  }

  .confetti {
    position:absolute;
    animation:confetti 1.9s linear forwards;
    pointer-events:none;
  }

  @keyframes confetti {
    to { transform:translateY(180vh) rotate(1440deg); opacity:0; }
  }
</style>
</head>
<body>

<div id="score-container">⭐ <span id="score">0</span></div>

<div id="loading">
  <div style="font-size:clamp(12rem,40vw,22rem);animation:bounce 1.6s infinite;">🎁</div>
  <div style="margin-top:2.5rem;font-size:clamp(2.2rem,7vw,4rem);letter-spacing:8px;font-family:'Orbitron',monospace; color:#b0b0ff;">
    LOFFI<span style="color:#d0c050;">GIFT</span>
  </div>
</div>

<div class="screen active" id="home">
  <div class="gift" id="gift">🎁</div>
</div>

<div class="screen" id="roulette">
  <div class="roulette-container">
    <div class="roulette-pointer"></div>
    <div class="roulette-strip auto-spin" id="rouletteStrip">
      <div class="roulette-item">❌</div><div class="roulette-item">🧸</div>
      <div class="roulette-item">🎁</div><div class="roulette-item">🚀</div>
      <div class="roulette-item">💍</div>
      <div class="roulette-item">❌</div><div class="roulette-item">🧸</div>
      <div class="roulette-item">🎁</div><div class="roulette-item">🚀</div>
      <div class="roulette-item">💍</div>
    </div>
  </div>

  <button id="spinBtn">Крутить (1000 ⭐)</button>
  <div id="result" style="margin-top:2.4rem;font-size:1.8rem;min-height:3.5rem;font-weight:600;"></div>
</div>

<div class="screen" id="inventory">
  <div class="balance" style="margin-bottom:3rem;">Инвентарь</div>
  <div id="items" style="font-size:1.7rem;line-height:1.9;text-align:center;max-width:760px;"></div>
</div>

<div class="nav">
  <button data-screen="home">🎁 Подарок</button>
  <button data-screen="roulette">🎰 Рулетка</button>
  <button data-screen="inventory">🎒 Инвентарь</button>
</div>

<script>
let score = +localStorage.getItem("score") || 0;
let inventory = JSON.parse(localStorage.getItem("inventory") || "{}");
let isSpinning = false;

const scoreEl = document.getElementById("score");
const loading = document.getElementById("loading");
const gift = document.getElementById("gift");
const spinBtn = document.getElementById("spinBtn");
const result = document.getElementById("result");
const itemsDiv = document.getElementById("items");
const rouletteStrip = document.getElementById("rouletteStrip");

const prizes = ["❌", "🧸", "🎁", "🚀", "💍"];
const itemWidth = 186;

// Фоновые падающие звёзды
for(let i = 0; i < 45; i++) {
  let s = document.createElement("div");
  s.className = "bg-star";
  s.textContent = Math.random() > 0.5 ? "✦" : "✧";
  s.style.left = Math.random()*100 + "%";
  s.style.animationDuration = (16 + Math.random()*45) + "s";
  s.style.animationDelay = Math.random()*35 + "s";
  document.body.appendChild(s);
}

// Убираем загрузку
setTimeout(() => {
  loading.style.opacity = "0";
  setTimeout(() => loading.remove(), 800);
}, 1800);

function updateBalance() {
  scoreEl.textContent = score;
  localStorage.setItem("score", score);
  localStorage.setItem("inventory", JSON.stringify(inventory));
}

// Клик по подарку
gift.onclick = e => {
  if (!document.getElementById("home").classList.contains("active")) return;

  score++;
  updateBalance();
  gift.classList.add("jump");
  setTimeout(() => gift.classList.remove("jump"), 500);

  const count = 8 + Math.floor(Math.random()*3);
  for(let i = 0; i < count; i++) {
    const star = document.createElement("div");
    star.className = "click-star";
    star.textContent = ["✦","✧","★","✨"][Math.floor(Math.random()*4)];

    const offsetX = (Math.random()-0.5)*100;
    const offsetY = (Math.random()-0.5)*70;

    star.style.left = (e.clientX + offsetX) + "px";
    star.style.top  = (e.clientY + offsetY) + "px";

    const drift = (Math.random()-0.5)*320;
    star.style.setProperty("--dx", drift + "px");

    star.style.animationDelay = (Math.random()*0.4) + "s";
    star.style.animationDuration = (2.2 + Math.random()*1.2) + "s";

    document.body.appendChild(star);
    setTimeout(() => star.remove(), 3800);
  }
};

// Навигация
document.querySelectorAll(".nav button").forEach(btn => {
  btn.addEventListener("click", () => {
    document.querySelectorAll(".screen").forEach(s => s.classList.remove("active"));
    document.getElementById(btn.dataset.screen).classList.add("active");

    document.querySelectorAll(".nav button").forEach(b => b.classList.remove("active"));
    btn.classList.add("active");

    if (btn.dataset.screen === "inventory") {
      itemsDiv.innerHTML = "";
      let has = false;
      for (let k in inventory) {
        if (inventory[k] > 0) {
          itemsDiv.innerHTML += `<div style="margin:1.2rem 0;">${k} × ${inventory[k]}</div>`;
          has = true;
        }
      }
      if (!has) itemsDiv.textContent = "Пока пусто...";
    }
  });
});

// Рулетка
function stopAutoSpin() {
  rouletteStrip.classList.remove("auto-spin");
  rouletteStrip.style.animation = "none";
  const current = window.getComputedStyle(rouletteStrip).transform;
  rouletteStrip.style.transform = current;
}

function resumeAutoSpin() {
  setTimeout(() => {
    rouletteStrip.style.transform = "translateX(0)";
    void rouletteStrip.offsetWidth;
    rouletteStrip.classList.add("auto-spin");
  }, 800);
}

function getRandomPrizeIndex() {
  const r = Math.random()*100;
  if (r < 1)    return 4;
  if (r < 5)    return 3;
  if (r < 14)   return 2;
  if (r < 33)   return 1;
  return 0;
}

spinBtn.onclick = () => {
  if (isSpinning) return;
  if (score < 1000) {
    alert("Недостаточно звёздочек ⭐");
    return;
  }

  score -= 1000;
  updateBalance();

  isSpinning = true;
  spinBtn.disabled = true;
  result.textContent = "Крутим... 🎰";
  stopAutoSpin();

  const target = getRandomPrizeIndex();
  const patternLen = prizes.length;
  const fullSpins = 11 + Math.floor(Math.random()*8);
  const total = fullSpins * patternLen + target;

  const finalPos = -total * itemWidth + (window.innerWidth / 2 - itemWidth / 2);

  rouletteStrip.style.transition = "none";
  void rouletteStrip.offsetWidth;
  rouletteStrip.style.transition = "transform 7.4s cubic-bezier(0.05, 0.98, 0.18, 1.01)";
  rouletteStrip.style.transform = `translateX(${finalPos}px)`;

  setTimeout(() => {
    const won = prizes[target];
    result.innerHTML = won === "❌" 
      ? "😔 Ничего не выпало..." 
      : `🎉 Вы выиграли <b>${won}</b>!`;

    if (won !== "❌") {
      inventory[won] = (inventory[won] || 0) + 1;
      updateBalance();

      for (let i = 0; i < 50; i++) {
        let c = document.createElement("div");
        c.className = "confetti";
        c.textContent = ["🎉","✨","⭐","💫"][Math.floor(Math.random()*4)];
        c.style.left = Math.random()*100 + "%";
        c.style.top = "-60px";
        document.body.appendChild(c);
        setTimeout(() => c.remove(), 2000);
      }
    }

    isSpinning = false;
    spinBtn.disabled = false;
    resumeAutoSpin();
  }, 7600);
};

updateBalance();
document.querySelector('.nav button[data-screen="home"]').classList.add("active");
</script>

<style>
  .click-star {
    position: absolute;
    font-size: 2.2rem;
    color: #e0e0ff;
    text-shadow: 0 0 12px rgba(220,220,255,0.7);
    pointer-events: none;
    animation: clickStarFall 2.6s forwards cubic-bezier(0.25, 0.1, 0.25, 1);
    z-index: 15;
  }

  @keyframes clickStarFall {
    0%   { transform: translate(0, 0) scale(1);   opacity: 0.9; }
    30%  { transform: translate(var(--dx), -90px) scale(1.2); opacity: 1; }
    100% { transform: translate(var(--dx), 140vh) scale(0.3); opacity: 0; }
  }
</style>
</body>
</html>
