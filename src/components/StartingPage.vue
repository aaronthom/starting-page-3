<template>
  <div class="video-background">
    <video playsinline="playsinline" autoplay="autoplay" muted="muted" loop="loop">
      <source src="../assets/video/nature.mp4" type="video/mp4">
    </video>
    <div class="content">
     
    </div>
  </div>
</template>

<script setup>


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
}

</style>