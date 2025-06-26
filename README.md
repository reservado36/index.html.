# index.html.
flor
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Ramo Morado con Corazón 💜</title>
  <style>
    body, html { margin: 0; padding: 0; width: 100%; height: 100%; overflow: hidden; background: #111; }
    canvas { display: block; margin: auto; background: transparent; }
    #mensaje {
      position: absolute;
      bottom: 20px;
      width: 100%;
      text-align: center;
      color: #e0ccff;
      font-size: 1.8em;
      opacity: 0;
      animation: aparecer 3s ease 4s forwards;
      text-shadow: 0 0 10px #b388eb;
    }
    @keyframes aparecer { to { opacity: 1; } }
  </style>
</head>
<body>
  <canvas id="canvas" width="900" height="600"></canvas>
  <div id="mensaje">
    Perdón... 💜<br>
    Te quiero mucho.<br>
    Gracias por estar aquí.<br>
    ¡Eres increíble!
  </div>
  <script>
    const c = document.getElementById('canvas');
    const ctx = c.getContext('2d');
    const W = c.width, H = c.height;
    class Flor {
      constructor(x,y,r,p,delay){
        this.x=x; this.y=y; this.r=r; this.p=p; this.delay=delay;
        this.g=0;
      }
      dibujar(){
        if(this.delay>0){ this.delay--; return; }
        if(this.g<1) this.g += 0.015;
        for(let i=0;i<this.p;i++){
          const a = i*(2*Math.PI/this.p),
                x1 = this.x+Math.cos(a)*this.r*0.2,
                y1 = this.y+Math.sin(a)*this.r*0.2,
                x2 = this.x+Math.cos(a)*this.r*this.g,
                y2 = this.y+Math.sin(a)*this.r*this.g;
          ctx.strokeStyle = `hsl(${300+i*10},100%,75%)`;
          ctx.lineWidth=2.5;
          ctx.shadowColor=ctx.strokeStyle;
          ctx.shadowBlur=15;
          ctx.beginPath();
          ctx.moveTo(x1,y1);
          ctx.lineTo(x2,y2);
          ctx.stroke();
        }
        ctx.shadowColor="#ffffff";
        ctx.shadowBlur=10;
        ctx.fillStyle="#c084fc";
        ctx.beginPath();
        ctx.arc(this.x,this.y,8,0,2*Math.PI);
        ctx.fill();
      }
    }
    function dibujarCorazon(){
      ctx.save();
      ctx.translate(W/2,H/3);
      ctx.beginPath();
      ctx.moveTo(0,0);
      ctx.bezierCurveTo(-50,-50,-120,20,0,100);
      ctx.bezierCurveTo(120,20,50,-50,0,0);
      ctx.strokeStyle="#ff66cc";
      ctx.lineWidth=4;
      ctx.shadowColor="#ff66cc";
      ctx.shadowBlur=20;
      ctx.stroke();
      ctx.restore();
    }
    const flores = [];
    const baseY = H - 100;
    [-240,-180,-120,-60,0,60,120,180,240].forEach((dx,i)=>{
      flores.push(new Flor(W/2+dx,baseY,70,14,i*20));
    });
    function animar(){
      ctx.clearRect(0,0,W,H);
      flores.forEach(f=>f.dibujar());
      if(flores.every(f=>f.g>=1)) dibujarCorazon();
      requestAnimationFrame(animar);
    }
    animar();
  </script>
</body>
</html>
