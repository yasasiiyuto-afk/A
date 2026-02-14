<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>アルティメットクリッカー</title>
<style>
body{
    background:#0f0f1a;
    color:white;
    text-align:center;
    font-family:sans-serif;
}
h1{margin-top:15px;}
#score{font-size:36px;margin:20px;}
button{
    padding:10px 20px;
    margin:8px;
    border:none;
    border-radius:8px;
    background:#6a00ff;
    color:white;
}
button:hover{background:#8e2bff;}
.popup{
    position:absolute;
    color:yellow;
    font-weight:bold;
    animation:float 1s ease-out forwards;
}
@keyframes float{
    from{opacity:1;transform:translateY(0);}
    to{opacity:0;transform:translateY(-50px);}
}
#boss{
    margin-top:15px;
    color:red;
    font-weight:bold;
}
</style>
</head>
<body>

<h1>🔥 アルティメットクリッカー 🔥</h1>
<div id="score">0</div>

<button onclick="clickScore(event)">クリック！</button>
<button onclick="buyClick()">クリック強化</button>
<button onclick="buyAuto()">自動生成</button>
<button onclick="gacha()">🎰 ガチャ</button>

<div id="boss"></div>

<audio id="clickSound" src="https://www.soundjay.com/button/beep-07.wav"></audio>

<script>
let score=0;
let clickPower=1;
let autoLevel=0;
let clickCost=20;
let autoCost=10;
let bossHP=0;

function update(){
    document.getElementById("score").innerText="ポイント: "+Math.floor(score);
}

function clickScore(e){
    document.getElementById("clickSound").play();
    
    let gain=clickPower;
    score+=gain;

    showPopup(e,gain);

    if(bossHP>0){
        bossHP-=gain;
        if(bossHP<=0){
            score+=500;
            document.getElementById("boss").innerText="🎉 ボス撃破！+500";
            setTimeout(()=>{document.getElementById("boss").innerText="";},2000);
        }else{
            document.getElementById("boss").innerText="🔥 ボスHP: "+bossHP;
        }
    }else{
        if(Math.random()<0.05){
            bossHP=200;
            document.getElementById("boss").innerText="🔥 ボス出現！HP:200";
        }
    }

    update();
}

function buyClick(){
    if(score>=clickCost){
        score-=clickCost;
        clickPower++;
        clickCost=Math.floor(clickCost*1.6);
        update();
    }
}

function buyAuto(){
    if(score>=autoCost){
        score-=autoCost;
        autoLevel++;
        autoCost=Math.floor(autoCost*1.6);
        update();
    }
}

function gacha(){
    if(score>=100){
        score-=100;
        let r=Math.random();
        if(r<0.5){
            clickPower+=2;
            alert("⭐ レア！クリック+2");
        }else{
            clickPower+=5;
            alert("🌈 超レア！クリック+5");
        }
        update();
    }else{
        alert("100ポイント必要！");
    }
}

function showPopup(e,value){
    let pop=document.createElement("div");
    pop.className="popup";
    pop.innerText="+"+value;
    pop.style.left=e.pageX+"px";
    pop.style.top=e.pageY+"px";
    document.body.appendChild(pop);
    setTimeout(()=>{pop.remove();},1000);
}

setInterval(()=>{
    score+=autoLevel;
    update();
},1000);

update();
</script>

</body>
</html>