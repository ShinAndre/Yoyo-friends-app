<template>
  <section class="video-page">
   <BButton pill class="back-btn mb-3" @click="emit('back')">
  <span class="arrow">←</span> Indietro
</BButton>

    <YouTube :src="video.videoId" width="100%" height="240" />

    <h2 class="h5 mt-3">{{ video.title }}</h2>
    <p class="text-muted">{{ video.author }} · {{ video.views }} visualizzazioni</p>
    <p>{{ video.description }}</p>

    <CommentSection :comments="comments" @submit="emit('comment', $event)" />
  </section>
</template>

<script setup>
import YouTube from 'vue3-youtube'
import { BButton } from 'bootstrap-vue-next'
import CommentSection from './CommentSection.vue'

defineProps({
  video: { type: Object, required: true },
  comments: { type: Array, default: () => [] }
})
const emit = defineEmits(['back', 'comment'])
</script>

<style scoped>
  .back-btn {
  background-color: white;
  border: 1px solid rgba(143, 185, 168, 0.6);
  color: #2f3e46;
  font-weight: 600;
  padding: 0.4rem 1.1rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
  transition: background-color 0.2s, box-shadow 0.2s;
}
.back-btn:hover {
  background-color: #8FB9A8;
  border-color: #8FB9A8;
  color: white;
  box-shadow: 0 4px 10px rgba(143, 185, 168, 0.5);
}
.arrow {
  display: inline-block;
  transition: transform 0.2s;
}
.back-btn:hover .arrow {
  transform: translateX(-3px);
}
</style>