<template>
  <div class="video-background">
    <video playsinline="playsinline" autoplay="autoplay" muted="muted" loop="loop">
      <source src="../assets/video/ocean-uhd.mp4" type="video/mp4">
    </video>
    <div class="content">
      <div class="date">{{ date }}</div>
      <div class="clock">{{ time }}</div>
      <h1 v-if="greeting === 'morning'">"Good morning!"</h1>
      <h1 v-if="greeting === 'afternoon'">"Good afternoon!"</h1>
      <h1 v-if="greeting === 'evening'">"Good evening!"</h1>
      <form class="search-form" @submit.prevent="searchGoogle">
        <input type="text" v-model="query" placeholder="Search Google..." class="search-input" />
        <button type="submit" class="search-button">Search</button>
      </form>
      <div class="icon-links">
        <a href="https://www.notion.so" target="_blank">
          <img src="..\assets\images\notion-logo-1000x1000.png">
        </a>
        <a href="https://www.github.com" target="_blank">
          <img src="..\assets\images\github-logo-1000x1000.png">
        </a>
        <a href="https://stackoverflow.com/" target="_blank">
          <img src="..\assets\images\stackoverflow-logo-1000x1000.png">
        </a>
        <a href="https://www.netflix.com" target="_blank">
          <img src="..\assets\images\netflix-logo-1000x1000.png">
        </a>
        <a href="https://www.chatgpt.com" target="_blank">
          <img src="..\assets\images\chatgpt-logo-1000x1000.png">
        </a>
        <a href="https://www.youtube.com" target="_blank">
          <img src="..\assets\images\youtube-logo-1000x1000.png">
        </a>
        <a href="https://www.amazon.com" target="_blank">
          <img src="..\assets\images\amazon-logo-1000x1000.png">
        </a>
      </div>
      <div class="quote">
        <p>{{ quote.content }}</p>
        <p><em>- {{ quote.author }}</em></p>
      </div>
    </div>
    <div v-if="!newsLoading" class="news-info">
      <h2>Latest News</h2>
      <ul>
        <li v-for="(article, index) in articles" :key="index">
          <h3>{{ article.title }}</h3>
          <p>{{ article.description }}</p>
          <a :href="article.url" target="_blank">Read more</a>
        </li>
      </ul>
    </div>
    <div v-else class="loading-message">Loading news...</div>
  </div>
  <div class="menu-icon" @click="toggleMenu">
    <img src="..\assets\images\menu-icon.png" alt="Menu">
  </div>
  <div v-if="isMenuOpen" class="side-menu">
    <div class="side-menu-header">
      <h2>Latest News</h2>
      <span @click="toggleMenu" class="close-button">&times;</span>
    </div>
    <div v-if="!newsLoading" class="news-info">
      <ul>
        <li v-for="(article, index) in articles" :key="index">
          <h3>{{ article.title }}</h3>
          <p>{{ article.description }}</p>
          <a :href="article.url" target="_blank">Read more</a>
        </li>
      </ul>
    </div>
    <div v-else class="loading-message">Loading news...</div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const query = ref('');
const time = ref('');
const date = ref('');
const greeting = ref('');
const isMenuOpen = ref(false);
const articles = ref([]);
const newsLoading = ref(true);
const quote = ref({ content: '', author: '' });

const updateTime = () => {
  const now = new Date();
  time.value = now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit', second: '2-digit' });
  date.value = now.toLocaleDateString('en-US', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' });

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

const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value;
};

const fetchNews = async () => {
  try {
    const response = await axios.get('https://newsapi.org/v2/top-headlines', {
      params: {
        country: 'us', // Change the country code as per your requirement
        apiKey: 'd8339343ec52462f836cb8cd0b45e9ce' // Replace with your actual API key
      }
    });
    articles.value = response.data.articles;
    newsLoading.value = false;
  } catch (error) {
    console.error('Error fetching news:', error);
    newsLoading.value = false;
  }
};

const fetchQuote = async () => {
  try {
    const response = await axios.get('https://api.quotable.io/random');
    quote.value = response.data;
  } catch (error) {
    console.error('Error fetching quote:', error);
  }
};

onMounted(() => {
  updateTime();
  setInterval(updateTime, 1000);
  fetchQuote();
  fetchNews();
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
  margin-top: 100px;
}

.date {
  font-size: 20px;
  font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
  font-weight: 500;
  margin-bottom: 10px;
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
  border: 1px solid #cccccc23;
  background-color: #ffffff18;
  border-radius: 20px 0 0 0;
  outline: none;
  color: white;
}

.search-button {
  padding: 10px 20px;
  font-size: 16px;
  border: 1px solid #cccccc23;
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
  margin-bottom: 0px;
  width: 700px;
}

.icon-links a img {
  border-radius: 60px;
}

.icon-links a img:hover {
  background-color: #cfd3d827;
  transition: all 200ms ease-in-out;
}

.menu-icon {
  position: absolute;
  top: 20px;
  right: 20px;
  z-index: 2;
  cursor: pointer;
}

.menu-icon img {
  width: 30px;
  height: 30px;
}

.side-menu {
  position: absolute;
  top: 0;
  right: 0;
  width: 700px;
  max-height: calc(100vh); /* 40px für den Abstand des Headers */
  overflow-y: auto; /* Falls die Höhe des Inhalts die maximale Höhe überschreitet */
  background-color: rgba(0, 0, 0, 0.8);
  color: white;
  z-index: 3;
  display: flex;
  flex-direction: column;
  padding: 20px;
}

.side-menu-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.close-button {
  margin-top: -24px;
  font-size: 46px;
  cursor: pointer;
}

.news-info {
  margin-top: 20px;
}

.news-info h3 {
  font-size: 22px;
  margin: 10px 0 5px;
}

.news-info p {
  margin: 5px 0;
}

.news-info a {
  color: #005bb5;
  margin: 10px 0 5px;
}

.quote {
  margin-top: 200px;
  text-align: center;
  font-size: 18px;
  font-family: 'Georgia', serif;
  font-style: italic;
}

.quote p {
  margin: 10px 0;
}
</style>
