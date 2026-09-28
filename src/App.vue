<script setup>
import { ref, nextTick, onMounted } from "vue";
import ChatMessage from "./components/ChatMessage.vue";

const messages = ref([]);
const input = ref("");
const typing = ref(false);
const list = ref(null);

const SUGGESTOES = ["Oi!", "Qual seu nome?", "Que horas são?", "Me conta uma piada", "Preço do plano?"];

const REGRAS = [
  { match: /\b(oi|ol[áa]|eai|opa|fala)\b/i, respostas: ["Olá! Como posso ajudar? 👋", "Oi! Tudo bem?", "E aí! Em que posso ser útil?"] },
  { match: /\b(nome|quem [ée] voc[êe])\b/i, respostas: ["Sou o BotZito, seu assistente de demo! 🤖", "Pode me chamar de BotZito."] },
  { match: /\b(hora|hor[áa]rio)\b/i, respostas: ["São " + new Date().toLocaleTimeString("pt-BR") + " ⏰"] },
  { match: /\b(ajuda|socorro|help)\b/i, respostas: ["Posso falar sobre preços, piadas e saudações. Experimente!"] },
  { match: /\b(pre[çc]o|quanto|custo|valor|plano)\b/i, respostas: ["O plano básico custa R$19/mês, o Pro R$49. Qual te interessa? 💰"] },
  { match: /\b(piada|engraçado|risada)\b/i, respostas: ["Por que o JS foi ao terapeuta? Porque perdeu o this! 😄", "Qual o favorito do programador? Café com ponto e vírgula ☕"] },
  { match: /\b(obrigad|valeu|brigad)\b/i, respostas: ["De nada! 😊", "Disponha!"] },
];

function hora() {
  return new Date().toLocaleTimeString("pt-BR", { hour: "2-digit", minute: "2-digit" });
}

function responder(texto) {
  for (const regra of REGRAS) {
    if (regra.match.test(texto)) {
      return regra.respostas[Math.floor(Math.random() * regra.respostas.length)];
    }
  }
  return "Interessante! Pode reformular? Tente perguntar sobre nome, hora, preço ou piada.";
}

async function scrollDown() {
  await nextTick();
  if (list.value) list.value.scrollTop = list.value.scrollHeight;
}

function enviar() {
  const texto = input.value.trim();
  if (!texto) return;
  messages.value.push({ autor: "user", texto, hora: hora() });
  input.value = "";
  scrollDown();
  typing.value = true;
  const delay = 800 + Math.random() * 800;
  setTimeout(() => {
    typing.value = false;
    messages.value.push({ autor: "bot", texto: responder(texto), hora: hora() });
    scrollDown();
  }, delay);
}

function sugerir(s) {
  input.value = s;
  enviar();
}

onMounted(() => {
  messages.value.push({ autor: "bot", texto: "Oi! Sou o BotZito. Pergunte sobre nome, hora, preço ou piada! 😊", hora: hora() });
  scrollDown();
});
</script>

<template>
  <main class="app">
    <div class="chat">
      <header class="chat__head">
        <div class="avatar">🤖</div>
        <div>
          <strong>BotZito</strong>
          <span class="status">● online</span>
        </div>
      </header>
      <div ref="list" class="chat__list">
        <TransitionGroup name="msg">
          <ChatMessage
            v-for="(m, i) in messages"
            :key="i"
            :autor="m.autor"
            :texto="m.texto"
            :hora="m.hora"
          />
        </TransitionGroup>
        <div v-if="typing" class="typing">
          <span></span><span></span><span></span>
        </div>
      </div>
      <div class="sugestoes">
        <button v-for="s in SUGESTOES" :key="s" @click="sugerir(s)">{{ s }}</button>
      </div>
      <div class="chat__input">
        <label for="chat-input" class="sr-only">Mensagem</label>
        <input
          id="chat-input"
          v-model="input"
          placeholder="Digite uma mensagem..."
          @keydown.enter="enviar"
        />
        <button @click="enviar">➤</button>
      </div>
    </div>
  </main>
</template>
