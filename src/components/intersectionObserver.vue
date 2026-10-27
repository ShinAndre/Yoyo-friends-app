<template>
  <div ref="sentinel" class="observer"></div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

const emit = defineEmits(['visible'])
const sentinel = ref(null)
let observer

onMounted(() => {
  observer = new IntersectionObserver(([entry]) => {
    if (entry.isIntersecting) emit('visible')
  })
  observer.observe(sentinel.value)
})

onBeforeUnmount(() => observer.disconnect())
</script>

<style>
.observer {
  height: 10px;
  grid-column: 1 / -1;
}
</style>