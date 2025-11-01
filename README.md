<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>EzzyAi — Offline Science Helper</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap" rel="stylesheet">
<style>
  body{
    margin:0;
    background:#0b1220;
    color:#e6eef6;
    font-family:'Inter',sans-serif;
    display:flex;
    flex-direction:column;
    height:100vh;
  }
  header{
    background:#0f172a;
    color:white;
    display:flex;
    align-items:center;
    padding:10px 16px;
    font-weight:600;
    font-size:18px;
    box-shadow:0 2px 6px rgba(0,0,0,0.4);
  }
  .logo{background:#0b84ff;padding:8px 14px;border-radius:8px;margin-right:10px;font-weight:700;}
  main{flex:1;display:flex;flex-direction:column;overflow:hidden;}
  #chat{
    flex:1;
    padding:14px;
    overflow-y:auto;
    display:flex;
    flex-direction:column;
    gap:10px;
  }
  .msg{
    max-width:80%;
    padding:10px 14px;
    border-radius:14px;
    line-height:1.4;
    box-shadow:0 2px 6px rgba(0,0,0,0.3);
  }
  .user{align-self:flex-end;background:#0b84ff;color:white;border-bottom-right-radius:6px;}
  .bot{align-self:flex-start;background:#f1f5f9;color:#071226;border-bottom-left-radius:6px;}
  footer{
    display:flex;
    padding:10px;
    background:#0f172a;
    box-shadow:0 -2px 6px rgba(0,0,0,0.3);
  }
  input{
    flex:1;
    padding:10px 12px;
    border-radius:10px;
    border:none;
    font-size:15px;
  }
  button{
    margin-left:8px;
    background:#0b84ff;
    color:white;
    font-weight:600;
    border:none;
    border-radius:10px;
    padding:10px 16px;
  }
  .typing-dots{display:flex;gap:4px;align-items:center;}
  .dot{width:6px;height:6px;border-radius:50%;background:#ccc;opacity:0.6;animation:blink 1s infinite;}
  .dot:nth-child(2){animation-delay:0.2s;}
  .dot:nth-child(3){animation-delay:0.4s;}
  @keyframes blink{0%,100%{opacity:0.3;}50%{opacity:1;}}
</style>
</head>
<body>
<header><div class="logo">E</div>EzzyAi — Offline Science Helper</header>
<main id="main">
  <div id="chat"></div>
</main>
<footer>
  <input id="input" type="text" placeholder="Ask EzzyAi... e.g. What is energy?" autocomplete="off" />
  <button id="send">Ask</button>
</footer>

<script>
const DATA=[
  {q:"what is work",a:"Work = Force × displacement in direction of force. Unit: Joule (J)."},
  {q:"what is power",a:"Power is the rate of doing work. Power = Work / Time. Unit: Watt (W)."},
  {q:"what is energy",a:"Energy is the capacity to do work. It exists as kinetic, potential, heat, etc."},
  {q:"what is ohm's law",a:"Ohm’s law: Voltage = Current × Resistance (V = I × R)."},
  {q:"what is photosynthesis",a:"Photosynthesis: Plants make food using sunlight, carbon dioxide, and water → glucose + oxygen."},
  {q:"what is acid",a:"Acids produce H⁺ ions in solution. They turn blue litmus red."},
  {q:"what is base",a:"Bases produce OH⁻ ions. They turn red litmus blue."},
  {q:"law of reflection",a:"Angle of incidence = angle of reflection."},
  {q:"speed of sound",a:"At 20°C in air ≈ 343 m/s."},
  {q:"newton's first law",a:"An object remains at rest or uniform motion unless acted on by an external force."},
  {q:"newton's second law",a:"Force = mass × acceleration (F = m × a)."},
  {q:"newton's third law",a:"Every action has an equal and opposite reaction."}
];

const chat=document.getElementById('chat');
const input=document.getElementById('input');
const send=document.getElementById('send');

function msg(txt,cls){
  const div=document.createElement('div');
  div.className='msg '+cls;
  div.innerHTML=txt;
  chat.appendChild(div);
  chat.scrollTop=chat.scrollHeight;
}

function typing(){
  const d=document.createElement('div');
  d.className='msg bot typing';
  d.innerHTML='<div class="typing-dots"><div class="dot"></div><div class="dot"></div><div class="dot"></div></div>';
  chat.appendChild(d);
  chat.scrollTop=chat.scrollHeight;
  return d;
}

function findAnswer(q){
  q=q.toLowerCase().trim();
  for(const i of DATA){
    if(i.q.includes(q) || q.includes(i.q)) return i.a;
  }
  return "Sorry, I don’t know that yet. Try asking about 'energy', 'acid', or 'photosynthesis'.";
}

async function handleAsk(){
  const q=input.value.trim();
  if(!q)return;
  msg("<strong>You:</strong> "+q,'user');
  input.value='';
  const t=typing();
  await new Promise(r=>setTimeout(r,600));
  t.remove();
  const a=findAnswer(q);
  msg("<strong>EzzyAi:</strong> "+a,'bot');
}

send.onclick=handleAsk;
input.addEventListener('keydown',e=>{
  if(e.key==='Enter'){e.preventDefault();handleAsk();}
});

msg("<strong>Hello, I'm EzzyAi!</strong><br>Ask me any 9th Std Science question — I work offline 🧠",'bot');
</script>
</body>
</html>

