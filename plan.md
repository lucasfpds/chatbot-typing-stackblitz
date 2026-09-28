# Chatbot com Animações de Digitação

> Interface de chat com "bot is typing" animado e balões de mensagem que deslizam.

## Stack

- Vite + Vue 3 (`<script setup>`, JavaScript) — sem TypeScript, sem lint/test, zero libs extras
- CSS puro em `src/styles.css`
- Arquivos: `index.html`, `package.json` (deps: `vue`), `vite.config.js`, `.stackblitzrc`, `src/main.js`, `src/App.vue`, `src/components/ChatMessage.vue`, `src/styles.css`

## Implementação

### 1. Layout

- Janela de chat: header (avatar, nome, status "online"), lista rolável de mensagens, input + botão enviar
- `components/ChatMessage.vue`: balão com prop `{ autor, texto, hora }` — usuário à direita, bot à esquerda, hora discreta em cada balão
- Auto-scroll para a última mensagem (`scrollTop = scrollHeight` no `nextTick` após cada mensagem)

### 2. Fluxo de mensagem

- Envio por Enter ou botão: balão do usuário entra imediatamente
- Resposta do bot: antes exibe o balão de "digitando" com **3 pontos pulsando** (keyframes com `animation-delay` escalonado), por 0.8–1.6s aleatório
- A resposta entra com slide — balões deslizam: usuário desliza da direita, bot da esquerda (`translateX` + `opacity` via `<TransitionGroup>`)

### 3. Cérebro do bot

- Motor de regras por palavra-chave em PT-BR (saudação, nome/hora, ajuda, preço, piada, obrigado…), com variações aleatórias por intenção e fallback educado
- Sugestões de perguntas clicáveis acima do input para guiar a demo

## Checklist — 100% da descrição

- [ ] Interface de chat com balões (usuário/bot) e horas
- [ ] Indicador "bot is typing" com pontos animados
- [ ] Balões deslizando ao entrar (direções distintas)
- [ ] Bot responde com regras simples + delay simulado
