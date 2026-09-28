<script setup>
defineProps({
  autor: String,
  texto: String,
  hora: String,
});
</script>

<template>
  <div class="msg" :class="autor === 'user' ? 'msg--user' : 'msg--bot'">
    <div class="bubble">
      <p>{{ texto }}</p>
      <span class="hora">{{ hora }}</span>
    </div>
  </div>
</template>
