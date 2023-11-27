<script setup>
import VideoCard from './components/VideoCard.vue';
import CommentSection from './components/CommentSection.vue';
import UsernameSpace from './components/UsernameSpace.vue';
</script>

<template>
  <div class="lab">
    <div class="videocard">
      <VideoCard />
    </div>
    <div class="commentsection">
      <UsernameSpace />
      <CommentSection />
    </div>
  </div>
</template>

<style scoped>
.videocard {
  width: 50vw;
  height: 100vh;
}

.commentsection {
  width: 50vw;
  height: 100vh;
  background-color: #f2f2f2;
}

.lab {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
</style>
