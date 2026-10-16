
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Mercadinho Divertido</title>
<style>
body{margin:0;font-family:Arial,sans-serif;background:#dceeff;color:#17345b;text-align:center}
h1{color:#175ca8}
main{max-width:850px;margin:20px auto;padding:20px}
#mercado{height:300px;position:relative;overflow:hidden;border:4px solid #2878d0;border-radius:18px;background:linear-gradient(#bde6ff 0 60%,#f7dba2 60%)}
.placa{position:absolute;top:15px;left:0;right:0;font-size:24px;font-weight:bold}
.prateleira{position:absolute;top:85px;left:5%;width:90%;height:65px;background:#a76a38;border:5px solid #75451f;display:flex;justify-content:space-around;align-items:center;font-size:30px}
#personagem{position:absolute;bottom:20px;left:5%;font-size:42px;transition:left .2s}
#carrinho{position:absolute;bottom:20px;right:8%;font-size:42px}
button{background:#2878d0;color:white;border:0;border-radius:12px;padding:13px 20px;margin:6px;font-size:17px;cursor:pointer}
button:hover{background:#175ca8}
#pergunta{font-size:22px;font-weight:bold;margin:20px}
#respostas button{display:block;width:90%;max-width:350px;margin:10px auto}
#mensagem{font-weight:bold;min-height:25px}
</style>
</head>
<body>
<main>
<h1>🛒 Mercadinho Divertido</h1>
<p>Use as setas ⬅️ ➡️ para andar pelo mercado!</p>

<div id="mercado">
  <div class="placa">🏪 SUPERMERCADO</div>
  <div class="prateleira">🍎 🥛 🍞 🧀 🍌</div>
  <div id="personagem">🧍</div>
  <div id="carrinho">🛒</div>
</div>

<button onclick="andar(-1)">⬅️ Esquerda</button>
<button onclick="andar(1)">Direita ➡️</button>

<h2 id="status">Encontre o carrinho!</h2>
<p id="pontos">⭐ Pontos: 0</p>
<button onclick="comprar()">🛍️ Fazer uma compra</button>

<div id="jogo" hidden>
  <p id="pergunta"></p>
  <div id="respostas"></div>
  <p id="mensagem"></p>
  <button id="proximo" onclick="proxima()" hidden>Próxima compra ➡️</button>
</div>
</main>

<script>
let posicao = 5;
let pontos = 0;
let indice = 0;
let respondeu = false;

const perguntas = [
 {q:"🍎 Uma maçã custa R$ 3. Quanto custam 2?",a:["R$ 5","R$ 6","R$ 9"],c:1},
 {q:"🥛 Leite custa R$ 5 e pão R$ 4. Total?",a:["R$ 8","R$ 9","R$ 10"],c:1},
 {q:"🍌 Você paga R$ 10 por bananas de R$ 7. Troco?",a:["R$ 2","R$ 3","R$ 4"],c:1},
 {q:"🍪 Três biscoitos custam R$ 4 cada. Total?",a:["R$ 7","R$ 12","R$ 14"],c:1},
 {q:"🧃 Um suco custa R$ 6. Você paga R$ 10. Troco?",a:["R$ 2","R$ 4","R$ 6"],c:1}
];

function andar(direcao){
 posicao=Math.max(2,Math.min(88,posicao+direcao*5));
 document.getElementById("personagem").style.left=posicao+"%";
 if(posicao>=73){
  document.getElementById("status").textContent="🎉 Você chegou ao carrinho!";
 }else{
  document.getElementById("status").textContent="🚶 Continue andando pelo mercado!";
 }
}

document.addEventListener("keydown",e=>{
 if(e.key==="ArrowLeft") andar(-1);
 if(e.key==="ArrowRight") andar(1);
});

function comprar(){
 if(posicao<65){
  document.getElementById("status").textContent="Ande para a direita até o carrinho primeiro!";
  return;
 }
 if(indice>=perguntas.length){
  document.getElementById("pergunta").textContent="🎉 Compras finalizadas!";
  document.getElementById("respostas").innerHTML="";
  document.getElementById("mensagem").textContent="Você fez "+pontos+" pontos de "+(perguntas.length*10)+"!";
  document.getElementById("jogo").hidden=false;
  return;
 }
 document.getElementById("jogo").hidden=false;
 mostrarPergunta();
}

function mostrarPergunta(){
 respondeu=false;
 const p=perguntas[indice];
 document.getElementById("pergunta").textContent=p.q;
 document.getElementById("mensagem").textContent="";
 document.getElementById("proximo").hidden=true;
 const area=document.getElementById("respostas");
 area.innerHTML="";
 p.a.forEach((resposta,i)=>{
  const b=document.createElement("button");
  b.textContent=resposta;
  b.onclick=()=>{
   if(respondeu)return;
   respondeu=true;
   if(i===p.c){
    pontos+=10;
    document.getElementById("mensagem").textContent="✅ Acertou! +10 pontos!";
   }else{
    document.getElementById("mensagem").textContent="🙂 Quase! A resposta certa é "+p.a[p.c]+".";
   }
   document.getElementById("pontos").textContent="⭐ Pontos: "+pontos;
   document.getElementById("proximo").hidden=false;
   document.getElementById("proximo").textContent=indice===perguntas.length-1?"Ver resultado 🏆":"Próxima compra ➡️";
  };
  area.appendChild(b);
 });
}

function proxima(){
 if(!respondeu)return;
 indice++;
 if(indice>=perguntas.length){
  document.getElementById("pergunta").textContent="🎉 Você terminou as compras!";
  document.getElementById("respostas").innerHTML="";
  document.getElementById("mensagem").textContent="Sua pontuação: "+pontos+" de "+(perguntas.length*10)+" pontos!";
  document.getElementById("proximo").hidden=true;
 }else{
  mostrarPergunta();
 }
}
</script>
</body>
</html>

