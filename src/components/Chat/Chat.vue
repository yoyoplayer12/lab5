<script setup>
    import { ref, reactive, onMounted } from 'vue'
    let message = ref(''); // int, string, boolean (neemt bij typen value op)
    let allMessages = reactive({
        data: ["Hej Hej!", "Hej Hejjjj!", "Hej HEEEEEEJ!"],
    }); // array, object
    const sendMessage = () =>{
        allMessages.data.push(message.value);
        message.value = '';
    };
</script>

<template>
    <div>
        <ul>
            <li><h4>Username:</h4></li>
            <li v-for="m in allMessages.data"><p>{{ m }}</p></li>
        </ul>
        <div>
            <input v-model="message" type="text" name="Comment" placeholder="Aa" id="">
            <button @click="sendMessage">Send</button>
        </div>
    </div>
</template>

<style scoped>

</style>
