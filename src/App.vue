<script setup>
import VideoPlayer from './components/Video/VideoPlayer.vue'
import VideoDetails from './components/Video/VideoDetails.vue'
import Chat from './components/Chat/Chat.vue'
import { ref } from 'vue';

let newTitle = ref('Default Title');

const updateVideo = (title) =>{
  newTitle.value = title;
}
//listen for emit description

</script>

<template>
  <div class="content">
    <div>
      <VideoPlayer @update:video-title="updateVideo" />
    </div>
    <div>
      <VideoDetails :title="newTitle" />
      <Chat />
    </div>
  </div>
</template>

<style scoped>
  .content{
    display: grid;
    grid-template-columns: 1fr 1fr;
  }
</style>