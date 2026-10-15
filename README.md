# Headlessscript
Site.headlessscriptNoKey
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="theme-color" content="#050000">
<title>HeadlessScript</title>

<style>
*{box-sizing:border-box}
body{
  margin:0;min-height:100vh;padding:18px;
  display:flex;align-items:center;justify-content:center;
  background:radial-gradient(ellipse,#30000b,#080204 65%,#000);
  color:#eee;font-family:Consolas,monospace
}
.painel{
  width:100%;max-width:650px;padding:24px;
  background:#090708;border:1px solid #a00025;
  box-shadow:0 0 45px #ff002533
}
.topo{
  display:flex;justify-content:space-between;gap:10px;
  flex-wrap:wrap;padding-bottom:15px;
  border-bottom:1px solid #40101a;
  color:#ff173d;font-size:12px;letter-spacing:1px
}
h1{font-size:clamp(27px,7vw,42px);overflow-wrap:anywhere}
.red{color:#ff173d;text-shadow:0 0 15px #ff173d55}
.texto{color:#aaa;font-size:13px;line-height:1.8}
.tag{
  display:inline-block;margin:25px 0 10px;padding:7px 10px;
  border:1px solid #ff173d;color:#ff173d;font-size:11px
}
.alerta{
  padding:16px;margin:22px 0;background:#21040a;
  border-left:4px solid #ff173d;line-height:2;font-size:13px
}
.fundo{height:9px;background:#292024;margin:12px 0}
.barra{
  width:0;height:100%;background:#ff173d;
  box-shadow:0 0 15px #ff173d;transition:width .15s
}
.porcentagem{text-align:right;color:#ff173d}
.registro{
  height:140px;overflow:auto;padding:12px;margin:20px 0;
  border:1px solid #302027;background:#030303;
  color:#aaa;font-size:12px;line-height:1.9;overflow-wrap:anywhere
}
.linha-alerta{color:#ff173d}
button{
  width:100%;padding:16px 10px;border:1px solid #ff173d;
  background:#820019;color:white;font-family:inherit;
  font-weight:bold;cursor:pointer;position:relative;z-index:2
}
button:hover{background:#b00025;box-shadow:0 0 20px #ff173d55}
button:disabled{opacity:.7;cursor:wait}
.rodape{margin-top:20px;color:#655b5e;font-size:10px;text-align:center}
.oculto{display:none!important}
.resultado{
  border:1px solid #ff173d;background:#21030a;
  padding:20px;margin:20px 0
}
.simbolo{font-size:55px;color:#ff173d;animation:piscando .4s infinite alternate}
#final{animation:aparecer .15s}
#flash{
  position:fixed;inset:0;background:#ff001c;
  pointer-events:none;z-index:10;opacity:0
}
#flash.ativo{animation:flash .55s ease-out}
#som{font-size:11px;color:#aaa;text-align:center;margin-top:12px}
@keyframes piscando{to{opacity:.4}}
@keyframes aparecer{from{transform:scale(.98);opacity:.4}to{transform:scale(1);opacity:1}}
@keyframes flash{
  0%{opacity:0}10%{opacity:.8}25%{opacity:.05}
  40%{opacity:.65}100%{opacity:0}
}
@media(max-width:450px){.painel{padding:17px}body{padding:10px}}
@media(prefers-reduced-motion:reduce){
  *,*::before,*::after{animation:none!important;transition:none!important}
}
</style>
</head>

<body>
<div id="flash"></div>

<main class="painel">
  <header class="topo">
    <span>[ HS ] HEADLESSSCRIPT</span>
    <span>● SISTEMA ATIVO</span>
  </header>

  <section id="inicio">
    <div class="tag">MONITORAMENTO DE SEGURANÇA</div>
    <h1>SISTEMA <span class="red">COMPROMETIDO</span></h1>

    <p class="texto">
      Uma atividade suspeita foi identificada.
      Inicie a análise para verificar o estado do sistema.
    </p>

    <div class="alerta">
      <strong class="red">⚠ ALERTA DE SEGURANÇA</strong><br>
      Nível de ameaça: <span class="red">CRÍTICO</span><br>
      Estado do sistema: <span class="red">INSTÁVEL</span><br>
      Análise: aguardando início
    </div>

    <div>PROGRESSO DA ANÁLISE</div>
    <div class="fundo"><div class="barra" id="barra"></div></div>
    <div class="porcentagem" id="porcentagem">0%</div>

    <div class="registro" id="registro" aria-live="polite">
      <div>[HS] Sistema inicializado.</div>
      <div>[HS] Aguardando comando...</div>
    </div>

    <button id="iniciar">INICIAR ANÁLISE</button>
    <div id="som">🔊 EFEITOS SONOROS ATIVADOS AO INICIAR</div>

    <div class="rodape">HEADLESSSCRIPT // MONITOR DE SEGURANÇA</div>
  </section>

  <section id="final" class="oculto" aria-live="assertive">
    <div class="tag">ALERTA CRÍTICO</div>
    <div class="simbolo">⚠</div>
    <h1 class="red">THREAT DETECTED</h1>

    <div class="resultado">
      <h2 class="red">⚠ SEUS DADOS FORAM ROUBADOS!</h2>
      <p class="texto">
        Atividade crítica identificada.<br>
        Nível de ameaça: MÁXIMO
      </p>
      <p class="red">STATUS: COMPROMETIMENTO CRÍTICO</p>
      <p class="texto">
        Conexão: instável<br>
        Segurança: comprometida<br>
        Monitoramento: ativo
      </p>
    </div>

    <p class="texto">O sistema permanece sob monitoramento.</p>
    <button id="novamente">EXECUTAR NOVA ANÁLISE</button>
    <div class="rodape">HEADLESSSCRIPT // ALERTA DE SEGURANÇA</div>
  </section>
</main>

<script>
const $ = id => document.getElementById(id);
const inicio = $("inicio");
const final = $("final");
const iniciar = $("iniciar");
const novamente = $("novamente");
const barra = $("barra");
const porcentagem = $("porcentagem");
const registro = $("registro");
const flash = $("flash");

let audio = null;
let temporizador = null;
let temporizadorFinal = null;
let executando = false;

function iniciarAudio() {
  const AudioContext = window.AudioContext || window.webkitAudioContext;
  if (!AudioContext) return null;

  if (!audio) audio = new AudioContext();
  if (audio.state === "suspended") audio.resume();
  return audio;
}

// Cria um som sintetizado sem baixar arquivos externos.
function tocarJumpscare() {
  const ctx = iniciarAudio();
  if (!ctx) return;

  const agora = ctx.currentTime;
  const volume = ctx.createGain();
  volume.gain.setValueAtTime(0.0001, agora);
  volume.gain.exponentialRampToValueAtTime(0.22, agora + 0.025);
  volume.gain.exponentialRampToValueAtTime(0.0001, agora + 1.15);
  volume.connect(ctx.destination);

  // Ruído curto, com efeito de impacto.
  const tamanho = Math.floor(ctx.sampleRate * 0.85);
  const buffer = ctx.createBuffer(1, tamanho, ctx.sampleRate);
  const dados = buffer.getChannelData(0);

  for (let i = 0; i < tamanho; i++) {
    const queda = Math.exp(-i / (ctx.sampleRate * 0.19));
    dados[i] = (Math.random() * 2 - 1) * queda;
  }

  const ruido = ctx.createBufferSource();
  ruido.buffer = buffer;

  const filtro = ctx.createBiquadFilter();
  filtro.type = "lowpass";
  filtro.frequency.setValueAtTime(1800, agora);
  filtro.frequency.exponentialRampToValueAtTime(180, agora + 0.7);

  ruido.connect(filtro);
  filtro.connect(volume);
  ruido.start(agora);

  // Tom grave e dissonante.
  [65, 69, 130].forEach((frequencia, i) => {
    const oscilador = ctx.createOscillator();
    const ganho = ctx.createGain();

    oscilador.type = i === 2 ? "sawtooth" : "triangle";
    oscilador.frequency.setValueAtTime(frequencia, agora);
    oscilador.frequency.exponentialRampToValueAtTime(
      frequencia * 0.55, agora + 0.8
    );

    ganho.gain.setValueAtTime(i === 2 ? 0.10 : 0.18, agora);
    ganho.gain.exponentialRampToValueAtTime(0.0001, agora + 0.9);

    oscilador.connect(ganho);
    ganho.connect(ctx.destination);
    oscilador.start(agora);
    oscilador.stop(agora + 0.95);
  });

  flash.classList.remove("ativo");
  void flash.offsetWidth;
  flash.classList.add("ativo");
}

const mensagens = [
  "Iniciando análise do sistema...",
  "Verificando setores simulados...",
  "Analisando ambiente de demonstração...",
  "Atividade suspeita identificada...",
  "Calculando nível de ameaça...",
  "ALERTA CRÍTICO REGISTRADO!",
  "Análise concluída."
];

function adicionarRegistro(mensagem, alerta = false) {
  const linha = document.createElement("div");
  linha.textContent = "[HS] " + mensagem;
  if (alerta) linha.className = "linha-alerta";
  registro.appendChild(linha);
  registro.scrollTop = registro.scrollHeight;
}

function reiniciar() {
  clearInterval(temporizador);
  clearTimeout(temporizadorFinal);
  executando = false;

  barra.style.width = "0%";
  porcentagem.textContent = "0%";
  registro.replaceChildren();
  adicionarRegistro("Sistema inicializado.");
  adicionarRegistro("Aguardando comando...");

  inicio.classList.remove("oculto");
  final.classList.add("oculto");
  iniciar.disabled = false;
  iniciar.textContent = "INICIAR ANÁLISE";
}

iniciar.addEventListener("click", () => {
  if (executando) return;
  executando = true;

  // Desbloqueia o áudio após o clique do visitante.
  iniciarAudio();

  iniciar.disabled = true;
  iniciar.textContent = "ANALISANDO SISTEMA...";

  let progresso = 0;
  let ultimaMensagem = -1;

  temporizador = setInterval(() => {
    progresso = Math.min(100, progresso + Math.floor(Math.random() * 4) + 2);
    barra.style.width = progresso + "%";
    porcentagem.textContent = progresso + "%";

    const indice = Math.min(
      mensagens.length - 1,
      Math.floor(progresso / 15)
    );

    if (indice !== ultimaMensagem) {
      adicionarRegistro(mensagens[indice], progresso >= 55);
      ultimaMensagem = indice;
    }

    if (progresso >= 100) {
      clearInterval(temporizador);
      adicionarRegistro("THREAT DETECTED.", true);

      temporizadorFinal = setTimeout(() => {
        inicio.classList.add("oculto");
        final.classList.remove("oculto");
        executando = false;

        // O som acontece junto à aparição do alerta final.
        tocarJumpscare();
      }, 650);
    }
  }, 180);
});

novamente.addEventListener("click", reiniciar);
</script>
</body>
</html>
