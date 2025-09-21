<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PixelPulse Stream</title>
<style>
  body, html {
    margin: 0;
    padding: 0;
    overflow: hidden;
    background-color: #0a0a0a;
    font-family: Arial, sans-serif;
    color: #0ff;
  }
  canvas {
    display: block;
  }
  #counter {
    position: absolute;
    top: 20px;
    left: 50%;
    transform: translateX(-50%);
    font-size: 24px;
    text-align: center;
    background: rgba(0,0,0,0.5);
    padding: 10px 20px;
    border-radius: 10px;
  }
</style>
</head>
<body>
<div id="counter">PixelPulse Transactions: 0</div>
<canvas id="pixelCanvas"></canvas>

<script>
const canvas = document.getElementById('pixelCanvas');
const ctx = canvas.getContext('2d');
const counterDiv = document.getElementById('counter');

let width = canvas.width = window.innerWidth;
let height = canvas.height = window.innerHeight;

const pixelCount = 150;
const pixels = [];
let transactionCount = 0;

// رموز ميمية صغيرة
const memes = ["🚀","💎","🙌","🔥","🪙"];

// إنشاء النقاط العشوائية
for(let i=0; i<pixelCount; i++){
    pixels.push({
        x: Math.random() * width,
        y: Math.random() * height,
        radius: Math.random() * 3 + 1,
        dx: (Math.random() - 0.5) * 1.5,
        dy: (Math.random() - 0.5) * 1.5,
        alpha: Math.random(),
        color: `rgb(0,255,255)`
    });
}

// عرض رموز الميمية عند كل 10 معاملات
const activeMemes = [];

function animate() {
    ctx.fillStyle = 'rgba(10,10,10,0.3)';
    ctx.fillRect(0, 0, width, height);

    for(let i=0; i<pixels.length; i++){
        const p = pixels[i];
        ctx.beginPath();
        ctx.arc(p.x, p.y, p.radius, 0, Math.PI*2);
        ctx.fillStyle = p.color;
        ctx.fill();
        ctx.closePath();

        p.x += p.dx;
        p.y += p.dy;

        if(p.x < 0 || p.x > width) p.dx *= -1;
        if(p.y < 0 || p.y > height) p.dy *= -1;

        p.alpha += (Math.random() - 0.5) * 0.02;
        if(p.alpha > 1) p.alpha = 1;
        if(p.alpha < 0.1) p.alpha = 0.1;

        // نبضة ميمية مع تغيير لون النقطة
        if(Math.random() < 0.005){
            transactionCount++;
            counterDiv.innerText = `PixelPulse Transactions: ${transactionCount}`;
            p.radius = Math.random() * 5 + 2; 
            p.color = `rgb(${Math.random()*255},${Math.random()*255},${Math.random()*255})`;

            // إضافة رمز ميم كل 10 معاملات
            if(transactionCount % 10 === 0){
                activeMemes.push({
                    x: Math.random() * width,
                    y: Math.random() * height,
                    text: memes[Math.floor(Math.random() * memes.length)],
                    alpha: 1
                });
            }
        }
    }

    // رسم رموز الميم
    for(let i=activeMemes.length-1; i>=0; i--){
        const m = activeMemes[i];
        ctx.font = "24px Arial";
        ctx.fillStyle = `rgba(255,255,255,${m.alpha})`;
        ctx.fillText(m.text, m.x, m.y);
        m.y -= 0.5; // تتحرك للأعلى
        m.alpha -= 0.01; // تختفي تدريجيًا
        if(m.alpha <= 0) activeMemes.splice(i,1);
    }

    requestAnimationFrame(animate);
}

animate();

window.addEventListener('resize', () => {
    width = canvas.width = window.innerWidth;
    height = canvas.height = window.innerHeight;
});
</script>
</body>
</html>
