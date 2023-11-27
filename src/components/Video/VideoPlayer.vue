<script setup>
import { ref, reactive, onMounted } from 'vue'
//define emits
const emit = defineEmits(['update:videoTitle']);
let videoUrl = ref('');
let videoNumber = ref(0);
let Videos = reactive({
    data: [],
});
onMounted(async () => {
    const response = await fetch('https://lab5api.onrender.com/api/v1/comments/videos');
    const data = await response.json();
    if (data) {
        const video = data.data[0].videos[videoNumber.value];
        videoUrl.value = video.videoUrl;

        Videos.data = data.data[0].videos;
        //emit
        emit('update:videoTitle', Videos.data[0].title);
    }
});

const arrowUp = () => {
    if (videoNumber.value < Videos.data.length - 1) {
        videoNumber.value++;
        videoUrl.value = Videos.data[videoNumber.value].videoUrl;
    }
    else {
        videoNumber.value = 0;
        videoUrl.value = Videos.data[videoNumber.value].videoUrl;
    }
};

const arrowDown = () => {
    if (videoNumber.value > 0) {
        videoNumber.value--;
        videoUrl.value = Videos.data[videoNumber.value].videoUrl;
    }
    else {
        videoNumber.value = Videos.data.length - 1;
        videoUrl.value = Videos.data[videoNumber.value].videoUrl;
    }
};
</script>

<template>
    <div>
        <iframe id="video" :src="videoUrl + '?autoplay=1&mute=1&controls=1&loop=1&playlist=<videoId>'" frameborder="0"></iframe>
        <div id="buttons">
            <button @click="arrowUp">Next</button>
            <button @click="arrowDown">Previous</button>
        </div>
    </div>
</template>
<style scoped>
#video {
    height: 90vh;
    width: 487px;
}

button {
    height: 50px;
    width: 100px;
    margin: 20px;
    margin-left: 50px;
    border: 0px solid black;
    border-radius: 5px;
    background-color: rgb(187, 187, 187);
}

#buttons {
    height: 10vh;
}
button:hover{
    cursor: pointer;
}
</style>
