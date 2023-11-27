<script setup>
import { ref, reactive, onMounted, watch } from 'vue'

const props = defineProps(["url"]);
let message = ref(''); // int, string, boolean (neemt bij typen value op)
let videoUrl = ref(props.url);
let usernameData = ref('');
let TheNewusername = 'Anonymous';
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
const addComment = async () => {
    try {
        if (message.value) {
            const response = await fetch('https://lab5api.onrender.com/api/v1/comments', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({
                    text: message.value,
                    username: usernameData.value,
                    videoUrl: videoUrl.value,
                }),
            });
            if (!response.ok) {
                throw new Error(`HTTP error! status: ${response.status}`);
            }
            const data = await response.json();
            comments.data.unshift(data['data']['comment']);
            message.value = '';
        }
    }
    catch (error) {
        console.error('An error occurred while posting the comment:', error);
    }
};
const formatTimestamp = (timestamp) => {
    const commentDate = new Date(timestamp);
    const currentDate = new Date();
    const diffInMinutes = Math.floor((currentDate - commentDate) / 60000);

    if (diffInMinutes < 1) {
        return 'now';
    } else if (diffInMinutes < 60) {
        return `${diffInMinutes} minutes ago`;
    } else if (diffInMinutes < 1440) {
        const diffInHours = Math.floor(diffInMinutes / 60);
        return `${diffInHours} hours ago`;
    } else {
        return commentDate.toLocaleString(undefined, { year: 'numeric', month: 'long', day: 'numeric', hour: '2-digit', minute: '2-digit' });
    }
};
const updateUsername = () => {
    if (usernameData.value) {
        usernameData.value = usernameData.value.trim();
        TheNewusername = usernameData.value;
    }
    else {
        usernameData.value = '';
        TheNewusername = 'Anonymous';
    }
}
</script>

<template>
    <div>
        <h3>Chat</h3>
        <label for="username" class="usernamelabel">Enter a Username:</label>
        <input id="username" v-model="usernameData" @input="updateUsername" placeholder="Username" />
        <ul class="commentul">
            <div v-for="c in comments.data" class="comment">
                <li class="username">{{ c['username'] }}</li>
                <li>{{ c['text'] }}</li>
                <li class="timestamp">{{ formatTimestamp(c['timestamp']) }}</li>
            </div>

        </ul>
        <div>
            <input class="commentsend" v-model="message" @keyup.enter="addComment" type="text" name="Comment" placeholder="Aa" id="">
            <button @click="addComment">Send</button>
        </div>
    </div>
</template>

<style scoped>
h3,
p,
li {
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

.timestamp {
    font-size: 10px;
    color: grey;
}
.comment {
    padding-bottom: 15px;
}
.usernamelabel{
    margin-left: 10px;
    margin-right: 15px;
    font-weight: bold;
    font-family: Arial, Helvetica, sans-serif;
}
.commentul{
    background-color: rgb(224, 224, 224);
    border-radius: 15px;
    height:500px;
    width: 500px;
    word-wrap: break-word;
    padding: 30px;
    overflow: auto;
}
.username{
    font-weight: bold;
    font-size: 13px;
}
.commentsend{
    width: 400px;
}
</style>
