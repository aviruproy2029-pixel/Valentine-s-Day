<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Cinematic Valentine 💖🎆</title>
<link href="https://fonts.googleapis.com/css2?family=Quicksand:wght@400;600;700&display=swap" rel="stylesheet">

<style>
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family:'Quicksand',sans-serif;
}

body{
    height:100vh;
    display:flex;
    align-items:center;
    justify-content:center;
    overflow:hidden;
    background:linear-gradient(135deg,#ffd6e8,#ffe9f3,#fff6fb);
    transition:background 1.5s ease;
}

.night{
    background:radial-gradient(circle at bottom,#0f2027,#203a43,#2c5364);
}

.container{
    text-align:center;
    background:white;
    padding:35px 25px;
    border-radius:25px;
    box-shadow:0 15px 35px rgba(255,105,180,0.2);
    width:92%;
    max-width:380px;
    transition:all 0.6s ease;
    z-index:2;
}

h1{
    font-size:1.8rem;
    margin-bottom:15px;
    color:#ff4d88;
}

p{
    margin-bottom:20px;
    color:#555;
}

button{
    padding:12px 25px;
    border:none;
    border-radius:30px;
    font-size:1rem;
    cursor:pointer;
    margin:8px;
    transition:all 0.25s ease;
}

.primary{
    background:#ff4d88;
    color:white;
    box-shadow:0 8px 20px rgba(255,77,136,0.3);
    animation:pulse 2s infinite;
}

.secondary{
    background:#ffe6f2;
    color:#ff4d88;
}

@keyframes pulse{
    0%{transform:scale(1);}
    50%{transform:scale(1.05);}
    100%{transform:scale(1);}
}

.hidden{
    opacity:0;
    transform:scale(0.9);
    pointer-events:none;
}

.message{
    margin-top:15px;
    opacity:0;
    transition:opacity 0.6s ease;
    font-size:0.95rem;
}

canvas{
    position:fixed;
    top:0;
    left:0;
    pointer-events:none;
    z-index:1;
}

.burst{
    position:absolute;
    font-size:22px;
    animation:explode 1s forwards;
}

@keyframes explode{
    0%{opacity:1; transform:scale(0.5);}
    100%{opacity:0; transform:translateY(-150px) scale(2);}
}

.final-screen{
    position:fixed;
    top:0;
    left:0;
    width:100%;
    height:100%;
    display:flex;
    align-items:center;
    justify-content:center;
    background:rgba(0,0,0,0.7);
    color:white;
    font-size:2rem;
    z-index:3;
    opacity:0;
    transition:opacity 1s ease;
}
</style>
</head>

<body>

<canvas id="fireworks"></canvas>

<div class="container" id="screen">
    <h1>404 🥺</h1>
    <p>Oopsie! Page Not Found</p>
    <button class="primary" onclick="showValentine()">Refresh</button>
</div>

<div class="final-screen" id="finalScreen">
    💍 It's a Date! 💖
</div>

<script>
let noClicks=0;
let yesScale=1;

let canvas=document.getElementById('fireworks');
let ctx=canvas.getContext('2d');
canvas.width=window.innerWidth;
canvas.height=window.innerHeight;

const music=new Audio("https://assets.mixkit.co/music/preview/mixkit-love-romantic-soft-piano-103.mp3");
music.loop=true;
music.volume=0.4;

function popSound(){
    const audio=new Audio("https://assets.mixkit.co/sfx/preview/mixkit-game-ball-tap-2073.mp3");
    audio.volume=0.4;
    audio.play();
}

function showValentine(){
    popSound();
    music.play();
    const screen=document.getElementById("screen");
    screen.classList.add("hidden");

    setTimeout(()=>{
        screen.innerHTML=`
            <h1>Will you be my Valentine? 💔</h1>
            <button class="primary" id="yesBtn">YES</button>
            <button class="secondary" id="noBtn">NO</button>
            <div class="message" id="yesMessage">
                <p>By clicking YES you agree to:</p>
                <p>• Unlimited cuddles<br>
                • Lifetime supply of bad puns<br>
                • Mutual pizza stealing rights<br>
                Effective immediately! 🚀</p>
            </div>
        `;
        screen.classList.remove("hidden");

        document.getElementById("yesBtn").addEventListener("click",yesClicked);
        document.getElementById("noBtn").addEventListener("click",noClicked);
    },600);
}

function yesClicked(){
    popSound();
    document.body.classList.add("night");

    const message=document.getElementById("yesMessage");
    const yesBtn=document.getElementById("yesBtn");
    const noBtn=document.getElementById("noBtn");

    message.style.opacity=1;
    heartExplosion();

    setTimeout(()=>{
        yesBtn.style.display="none";
        noBtn.style.display="none";
    },1000);

    launchFireworks();

    setTimeout(()=>{
        document.getElementById("finalScreen").style.opacity=1;
    },5000);
}

function noClicked(){
    popSound();
    const noBtn=document.getElementById("noBtn");

    noBtn.style.position="absolute";
    noBtn.style.top=Math.random()*(window.innerHeight-60)+"px";
    noBtn.style.left=Math.random()*(window.innerWidth-120)+"px";

    noClicks++;
    yesScale+=0.15;
    document.getElementById("yesBtn").style.transform=`scale(${yesScale})`;

    if(noClicks>=2){
        document.querySelector("h1").innerHTML="Will you be my Valentine? 😭";
    }
}

function heartExplosion(){
    for(let i=0;i<30;i++){
        const heart=document.createElement("div");
        heart.classList.add("burst");
        heart.innerHTML="💗";
        heart.style.left=window.innerWidth/2+"px";
        heart.style.top=window.innerHeight/2+"px";
        document.body.appendChild(heart);
        setTimeout(()=>heart.remove(),1000);
    }
}

function launchFireworks(){
    let particles=[];

    for(let i=0;i<200;i++){
        particles.push({
            x:canvas.width/2,
            y:canvas.height/2,
            angle:Math.random()*2*Math.PI,
            speed:Math.random()*6+2,
            radius:Math.random()*3+1,
            life:120
        });
    }

    let animation=setInterval(()=>{
        ctx.fillStyle="rgba(0,0,0,0.2)";
        ctx.fillRect(0,0,canvas.width,canvas.height);

        particles.forEach(p=>{
            p.x+=Math.cos(p.angle)*p.speed;
            p.y+=Math.sin(p.angle)*p.speed;
            p.life--;

            ctx.beginPath();
            ctx.arc(p.x,p.y,p.radius,0,2*Math.PI);
            ctx.fillStyle=`hsl(${Math.random()*360},100%,60%)`;
            ctx.fill();
        });

        particles=particles.filter(p=>p.life>0);

        if(particles.length===0){
            clearInterval(animation);
            ctx.clearRect(0,0,canvas.width,canvas.height);
        }

    },30);
}

window.addEventListener("resize",()=>{
    canvas.width=window.innerWidth;
    canvas.height=window.innerHeight;
});
</script>

</body>
</html>