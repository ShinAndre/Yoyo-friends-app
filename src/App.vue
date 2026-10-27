<template>
  <BApp>
    <header class="header">
      <h1 class="title">Yo<span>Yo</span></h1>
      <span class="friends">Friends</span>
    </header>

    <main class="surface">
      <VideoList
        v-show="!selectedVideo"
        :videos="videos"
        @select="selectedVideo = $event"
      />
      <VideoPage
        v-if="selectedVideo"
        :video="selectedVideo"
        :comments="comments[selectedVideo.id] ?? []"
        @back="selectedVideo = null"
        @comment="addComment"
      />
    </main>
  </BApp>
</template>

<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";
import { BApp, BButton } from "bootstrap-vue-next";
import VideoList from "./components/VideoList.vue";
import VideoPage from "./components/VideoPage.vue";

const API =
  "https://raw.githubusercontent.com/Anita-Liberatore/yoyo-friends-api/master/videos.json";

const videos = ref([]);
const selectedVideo = ref(null);
const comments = ref({}); // { [videoId]: ['commento', ...] }

onMounted(async () => {
  const { data } = await axios.get(API);
  videos.value = data;
});

function addComment(text) {
  const id = selectedVideo.value.id;
  comments.value[id] = [...(comments.value[id] ?? []), text];
}
</script>

<style>
.header {
  position: sticky;
  top: 0;
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  gap: 0.75rem;
  padding: 0.75rem 1.25rem;
  background: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(10px);
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.06);
}
.title {
  margin: 0;
  font-size: 1.75rem;
  font-weight: 800;
  letter-spacing: -0.5px;
  color: #2f3e46;
}
.title span {
  color: #8fb9a8;
}
.friends {
  background-color: #8fb9a8 !important;
  border: none !important;
  color: white !important;
  font-weight: 600;
  padding: 0.35rem 1.1rem;
  cursor: default !important;
  border-radius: 50rem;
  transition:
    transform 0.2s,
    box-shadow 0.2s;
}
.friends:hover {
  transform: translateY(-1px);
  box-shadow: 0 4px 10px rgba(143, 185, 168, 0.5);
}
.surface {
  min-height: 100vh;
  padding: 1rem;
  background-color: rgb(251, 244, 235);
}
</style>
