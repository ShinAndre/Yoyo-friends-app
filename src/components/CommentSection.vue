<template>
  <div>
    <h3 class="h6">Commenti ({{ comments.length }})</h3>

    <ul class="list-unstyled">
      <li v-for="(comment, i) in comments" :key="i" class="comment">{{ comment }}</li>
    </ul>

    <form class="d-flex gap-2" @submit.prevent="send">
      <input v-model="text" class="form-control" placeholder="Scrivi un commento..." />
      <BButton type="submit" :disabled="!text.trim()">Invia</BButton>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { BButton } from 'bootstrap-vue-next'

defineProps({ comments: { type: Array, default: () => [] } })
const emit = defineEmits(['submit'])

const text = ref('')

function send() {
  emit('submit', text.value.trim())
  text.value = ''
}
</script>

<style scoped>
.comment {
  background: white;
  border-radius: 1rem;
  padding: 0.5rem 1rem;
  margin-bottom: 0.5rem;
}
</style>