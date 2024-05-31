<template>
  <div class="video-background">
    <video playsinline="playsinline" autoplay="autoplay" muted="muted" loop="loop">
      <source src="../assets/video/ocean-uhd.mp4" type="video/mp4">
    </video>
    <div class="content">
      <div class="clock">{{ time }}</div>
      <h1 v-if="greeting === 'morning'">Good morning, Aaron!</h1>
      <h1 v-if="greeting === 'afternoon'">Good afternoon, Aaron!</h1>
      <h1 v-if="greeting === 'evening'">Good evening, Aaron!</h1>
      <form class="search-form" @submit.prevent="searchGoogle">
        <input type="text" v-model="query" placeholder="Search Google..." class="search-input" />
        <button type="submit" class="search-button">Search</button>
      </form>
      <div class="icon-links">
        <a href="https://www.youtube.com" target="_blank"> 
          YouTube
        </a>
        <a href="https://www.netflix.com" target="_blank">
          Netflix
        </a>
        <a href="https://www.amazon.com" target="_blank">
          Amazon
        </a>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const query = ref('');
const time = ref('');
const greeting = ref('');

const updateTime = () => {
  const now = new Date();
  time.value = now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit', second: '2-digit' });

  const hours = now.getHours();
  if (hours >= 5 && hours < 12) {
    greeting.value = 'morning';
  } else if (hours >= 12 && hours < 18) {
    greeting.value = 'afternoon';
  } else {
    greeting.value = 'evening';
  }
};

const searchGoogle = () => {
  const searchUrl = `https://www.google.com/search?q=${encodeURIComponent(query.value)}`;
  window.location.href = searchUrl;
};

onMounted(() => {
  updateTime();
  setInterval(updateTime, 1000);
});
</script>

<style scoped>
.video-background {
  position: relative;
  width: 100%;
  height: 100vh;
  overflow: hidden;
}

.video-background video {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.content {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100%;
  color: white;
}

.clock {
  font-size: 58px;
  font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
  margin-bottom: 0px;
}

h1 {
  font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
  font-size: 28px;
  font-style: italic;
  font-weight: 400;
  margin-bottom: 20px;
}

.search-form {
  display: flex;
  align-items: center;
}

.search-input {
  width: 600px;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #cccccc42;
  background-color: #ffffff18;
  border-radius: 20px 0 0 0;
  outline: none;
  color: white;
}

.search-button {
  padding: 10px 20px;
  font-size: 16px;
  border: 1px solid #cccccc42;
  border-left: none;
  border-radius: 0 0 20px 0;
  background-color: hsl(209, 71%, 43%);
  color: white;
  cursor: pointer;
}

.search-button:hover {
  background-color: #005bb5;
  transition: 500ms ease-in-out;
}

.icon-links {
  display: flex;
  justify-content: center;
  margin-top: 20px;
  gap: 40px;
  margin-bottom: 200px;
}

.icon {
  font-size: 30px;
  margin: 0 10px;
  color: white;
  cursor: pointer;
}

.icon:hover {
  color: #ccc;
}
</style>
