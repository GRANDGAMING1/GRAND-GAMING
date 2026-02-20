# GRAND-GAMING<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Grand Gaming</title>
<style>
body{
  margin:0;
  font-family:'Segoe UI',sans-serif;
  color:white;
  overflow-x:hidden;
  background: linear-gradient(-45deg,#4a90e2,#50e3c2,#f5a623,#9013fe);
  background-size:400% 400%;
  animation:gradientBG 20s ease infinite;
}
@keyframes gradientBG{
  0%{background-position:0% 50%;}
  50%{background-position:100% 50%;}
  100%{background-position:0% 50%;}
}
.header{
  font-size:36px;
  text-align:center;
  margin-top:20px;
  color:#00ff88;
  text-shadow:0 0 20px #00ff88;
}
.container{
  max-width:400px;
  margin:20px auto;
  position:relative;
}
select,button{
  width:80%;
  padding:14px;
  margin:10px auto;
  display:block;
  font-size:18px;
  border-radius:12px;
  border:2px solid white;
  background:rgba(0,0,0,0.6);
  color:white;
  cursor:pointer;
  transition:0.3s;
}
button:hover{
  transform:scale(1.05);
  box-shadow:0 0 15px #00ff88;
}
.epic{border-color:orange;}
.showtime{border-color:#00aaff;}
.potw{border-color:#00ff88;}
.telegram{border-color:#0088cc;}
#timer{font-size:20px;color:#00ff88;text-align:center;margin-top:10px;}
#resultSection{
  margin-top:50px;
  padding:20px;
  display:none;
}
#circle{
  width:180px;
  height:180px;
  border-radius:50%;
  background: conic-gradient(#00ff88 0%, #111 0%);
  display:flex;
  align-items:center;
  justify-content:center;
  font-size:36px;
  margin:0 auto 15px auto;
  box-shadow:0 0 25px #00ff88;
}
#resultText{
  font-size:18px;
  text-align:center;
  text-shadow:0 0 15px #00ff88;
}
.hidden{display:none;}
</style>
</head>
<body>

<!-- واجهة 1 -->
<div id="screen1" class="container">
  <div class="header">Grand Gaming</div>
  <select id="device"><option disabled selected>اختر جهازك</option><option>Android</option><option>iPhone</option></select>
  <select id="server"><option disabled selected>اختر السيرفر</option><option>Asia</option><option>Europe</option><option>Africa</option></select>
  <button onclick="goToScreen2()">Confirm</button>
</div>

<!-- واجهة 2 -->
<div id="screen2" class="container hidden">
  <div class="header">Grand Gaming</div>
  <div id="timer">⏳ 02:00</div>
  <button class="epic" onclick="startScan('EPIC')">EPIC SCAN</button>
  <button class="showtime" onclick="startScan('SHOWTIME')">SHOWTIME SCAN</button>
  <button class="potw" onclick="startScan('POTW')">POTW SCAN</button>
  <button class="telegram" onclick="window.open('https://t.me/Grand_Gaming10')">Telegram</button>
  <div id="resultSection">
    <div id="circle">0%</div>
    <div id="resultText"></div>
  </div>
</div>

<audio id="scanSound" src="https://freesound.org/data/previews/341/341695_6247760-lq.mp3"></audio>

<script>
let scanCooldown=120000,lastScan=0,timerInterval,timerRemaining=0;
let scanSound=document.getElementById("scanSound");

function goToScreen2(){
  let d=document.getElementById("device").value,s=document.getElementById("server").value;
  if(!d||!s){alert("اختر الجهاز والسيرفر");return;}
  document.getElementById("screen1").classList.add("hidden");
  document.getElementById("screen2").classList.remove("hidden");
  startTimer();
}

function startTimer(){
  clearInterval(timerInterval);
  let now=Date.now();
  timerRemaining=lastScan>0?Math.max(0,(lastScan+scanCooldown-now)/1000):0;
  timerInterval=setInterval(()=>{
    let min=Math.floor(timerRemaining/60),
        sec=Math.floor(timerRemaining%60);
    document.getElementById("timer").innerText="⏳ "+min+":"+(sec<10?"0"+sec:sec);
    timerRemaining--; if(timerRemaining<0) timerRemaining=0;
  },1000);
}

// أغلب النسب بين 55 و 90، وبعض القيم أقل تظهر أحيانًا
function smartPercent(){
  return Math.random()<0.8 ? 55 + Math.floor(Math.random()*36) : 1 + Math.floor(Math.random()*54);
}

function startScan(type){
  let now=Date.now();
  if(now<lastScan+scanCooldown){alert("⏳ استنى قبل الفحص التالي!");return;}
  lastScan=now;startTimer();
  
  scanSound.currentTime=0;scanSound.play();

  let circle=document.getElementById("circle"),
      resultText=document.getElementById("resultText"),
      resultSection=document.getElementById("resultSection");
  resultSection.style.display="block";
  circle.innerText="0%";
  circle.style.background="conic-gradient(#00ff88 0%, #111 0%)";
  resultText.innerText="";

  let target=smartPercent(),current=0;
  let interval=setInterval(()=>{
    if(current>=target){
      clearInterval(interval);
      let text="";
      if(target>=90)text="🔥 قابل لفتح الباك";
      else if(target>=80)text="✅ مقبول بس مش مضمون، يُستحسن تفتحه مرة تانية";
      else if(target>=70)text="⚠️ مقبول، يُستحسن تجرب مرة أخرى لاحقًا";
      else if(target>=60)text="❌ نسبة منخفضة، يُستحسن تجرب لاحقًا";
      else if(target>=55)text="❌ نسبة منخفضة، يُستحسن تجرب لاحقًا";
      else if(target>=40)text="🚫 سئ للغاية، جرب لاحقًا";
      else if(target>=20)text="🚫 سئ للغاية، جرب لاحقًا";
      else text="⛔ سئ جدًا جدًا، لا تفتح، جرب لاحقًا";
      resultText.innerText=text;
    } else {
      current++;
      circle.innerText=current+"%";
      circle.style.background="conic-gradient(#00ff88 "+current+"%, #111 "+current+"%)";
    }
  },30);
}
startTimer();
</script>

</body>
</html>
