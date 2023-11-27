<script setup>
import { ref, reactive, onMounted, watch } from 'vue'
let message = ref(''); // int, string, boolean (neemt bij typen value op)

let videoUrl = ref('https://www.youtube.com/embed/dlI1AhNHqK0');
let comments = reactive({
    data: [],
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






// let allMessages = reactive({
//     data: ["Hej Hej!", "Hej Hejjjj!", "Hej HEEEEEEJ!"],
// }); // array, object
// const sendMessage = () =>{
//     allMessages.data.push(message.value);
//     message.value = '';
// };
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
            <input v-model="message" type="text" name="Comment" placeholder="Aa" id="">
            <button @click="sendMessage">Send</button>
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
