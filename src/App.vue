<script setup>
import VideoPlayer from './components/Video/VideoPlayer.vue'
import VideoDetails from './components/Video/VideoDetails.vue'
import Chat from './components/Chat/Chat.vue'
import { ref } from 'vue';

let newTitle = ref('');
let newUsername = ref('');
let newUrl = ref('');

const updateVideo = (title, username, url) =>{
  newTitle.value = title;
  newUsername.value = username;
  newUrl.value = url;
}
//listen for emit description

</script>

<template>
  <div class="content">
    <div>
      <VideoPlayer @update:video="updateVideo" />
    </div>
    <div>
      <VideoDetails :title="newTitle" :username="newUsername" />
      <Chat :url="newUrl" />
    </div>
  </div>
</template>

<style scoped>
  .content{
    display: grid;
    grid-template-columns: 1fr 1fr;
  }
</style>