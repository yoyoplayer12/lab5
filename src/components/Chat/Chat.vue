<script setup>
import { ref, reactive, onMounted, watch } from 'vue'

const props = defineProps(["url"]);
let message = ref(''); // int, string, boolean (neemt bij typen value op)
let videoUrl = ref(props.url);
let comments = reactive({
    data: [],
});
watch(() => props.url, (newUrl) => {
  videoUrl.value = newUrl;
  fetchComments();
});
const fetchComments = async () => {
    const response = await fetch(`https://lab5api.onrender.com/api/v1/comments/video/${encodeURIComponent(videoUrl.value)}`);
    const data = await response.json();
    console.log(data['data']['comment']);
    if (data['data']['comment'] == "There are no comments yet.") {
        data['data']['comment'] = [];
    }
    else {
        data['data']['comment'] = data['data']['comment'].reverse();
    }
    comments.data = data['data']['comment'];
};
onMounted(() => fetchComments());
watch(videoUrl, () => fetchComments());
const addComment = async() => {
    //   try {
    //     if (this.newComment) {
    //       const response = await fetch('https://lab5api.onrender.com/api/v1/comments', {
    //         method: 'POST',
    //         headers: {
    //           'Content-Type': 'application/json',
    //         },
    //         body: JSON.stringify({
    //           text: this.newComment,
    //           username: this.username,
    //           videoUrl: this.videoUrl,
    //         }),
    //       });
    //       if (!response.ok) {
    //         throw new Error(`HTTP error! status: ${response.status}`);
    //       }
    //       const data = await response.json();
    //       this.comments.unshift(data['data']['comment']);
    //       this.newComment = '';
    //     }
    //   }
    //   catch (error) {
    //     console.error('An error occurred while posting the comment:', error);
    //   }
    console.log('Adding comment');
    };
</script>

<template>
    <div>
        <h3>Chat</h3>
        <ul>
            <div v-for="c in comments.data" class="comment">
                <li>{{c['username']}}: {{ c['text'] }}</li>
                <li class="timestamp">{{ new Date(c['timestamp']).toLocaleString(undefined, {year: 'numeric', month: 'long', day: 'numeric', hour: '2-digit', minute: '2-digit'}) }}</li>
            </div>
            
        </ul>
        <div>
            <input v-model="message" @keyup.enter="addComment" type="text" name="Comment" placeholder="Aa" id="">
            <button @click="addComment">Send</button>
        </div>
    </div>
</template>

<style scoped>
h3,
p, li {
    font-family: Arial, Helvetica, sans-serif;
}

ul {
    list-style-type: none;
    padding-left: 10px;
    margin-top: 20px;
}

h3 {
    font-size: 30px;
    margin-top: 100px;
}
.timestamp{
    font-size: 10px;
    color: grey;
}
.comment{
    padding-bottom: 15px;
}
</style>
