<script setup lang="ts">
import { computed, onMounted, ref } from "vue";
import HelloWorld from "./components/HelloWorld.vue";

function startViewTransition(callback: () => void) {
  if (!document.startViewTransition) {
    callback();
    return;
  }
  document.startViewTransition(callback);
}

const state = ref(true);

const objectFit = computed(() => (state.value ? "contain" : "cover"));

const test = ref("cover");

const handleCLick = () => {
  startViewTransition(() => {
    document.documentElement.classList.toggle("cover");
    state.value = !state.value;
  });
};
</script>

<template>
  <button @click="handleCLick">点我</button>
  <header class="main-header">
    <span v-if="state">⬅️ </span>
    <h1>A short title</h1>
  </header>

  <div v-if="state" class="card">
    <img class="img1" src="./assets/1.png" alt="" />
  </div>

  <div v-else class="card1">
    <img class="img2" src="./assets/2.png" alt="" />
  </div>
</template>

<style>
/* 控制整体动画速度 */
::view-transition-group(root) {
  animation-duration: 10s;
}

@keyframes fade-in {
  from {
    opacity: 0;
  }
}

@keyframes fade-out {
  to {
    opacity: 0;
  }
}

@keyframes slide-from-right {
  from {
    transform: translateX(30px);
  }
}

@keyframes slide-to-left {
  to {
    transform: translateX(-30px);
  }
}

::view-transition-old(root) {
  animation: 500ms cubic-bezier(0.4, 0, 1, 1) both fade-out,
    500ms cubic-bezier(0.4, 0, 0.2, 1) both slide-to-left;
}

::view-transition-new(root) {
  animation: 500ms cubic-bezier(0, 0, 0.2, 1) 90ms both fade-in,
    500ms cubic-bezier(0.4, 0, 0.2, 1) both slide-from-right;
}
button {
  view-transition-name: button;
}
.main-header {
  view-transition-name: main-header;
}

h1 {
  view-transition-name: main-header-text;
  display: inline-block;
  width: fit-content;
}

.card {
  width: 300px;
  height: 300px;
  view-transition-name: full-embed;
}
.card img {
  object-fit: cover !important;
}
.card1 {
  width: 500px;
  view-transition-name: full-embed;
}
.card1 img {
  object-fit: cover !important;
}
img {
  width: 100%;
  height: 100%;
}
::view-transition-old(full-embed),
::view-transition-new(full-embed) {
  /* Prevent the default animation,
  so both views remain opacity:1 throughout the transition */
  /* animation: none; */
  /* Use normal blending,
  so the new view sits on top and obscures the old view */
  /* mix-blend-mode: normal; */
  /* Make the height the same as the group,
  meaning the view size might not match its aspect-ratio. */
  height: 100%;
  /* Clip any overflow of the view */
  overflow: clip;
}
/* The old view is the thumbnail */
::view-transition-old(full-embed) {
  object-fit: cover;
}

::view-transition-new(full-embed) {
  object-fit: contain;
}

html.cover::view-transition-old(full-embed) {
  object-fit: contain;
}

html.cover::view-transition-new(full-embed) {
  object-fit: cover;
}
</style>

<style scoped>
.logo {
  height: 12em;
  padding: 1.5em;
  will-change: filter;
}
</style>
