<template>
  <div class="video-list">
    <BCard
      v-for="video in visibleVideos"
      :key="video.id"
      :title="video.title"
      :img-src="video.thumbnail"
      :img-alt="video.title"
      img-top
      tag="article"
      class="video-card"
      @click="emit('select', video)"
    >
      <small class="text-muted">{{ video.author }} · {{ video.duration }}</small>
    </BCard>

    <ScrollSentinel
      v-if="visibleCount < videos.length"
      :key="visibleCount"
      @visible="visibleCount += 3"
    />
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import { BCard } from 'bootstrap-vue-next'
import ScrollSentinel from './intersectionObserver.vue'

const props = defineProps({ videos: { type: Array, default: () => [] } })
const emit = defineEmits(['select'])

const visibleCount = ref(3)
const visibleVideos = computed(() => props.videos.slice(0, visibleCount.value))
</script>

<style scoped>
.video-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
  gap: 1rem;
  max-width: 960px;
  margin: 0 auto;
}
.video-card {
  cursor: pointer;
}
.video-list > :last-child:is(.observer) {
  grid-column: 1 / -1;
}
</style>