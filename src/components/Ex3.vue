<script>
import axios from 'axios'

export default { 
    data() {
        return {
            subject: '',
            entry: '',
            mood: '',
            moods: ['Happy', 'Sad', 'Excited', 'Tired', 'Angry'],
            outputMsg: ''
        }
    },
    computed: {
        baseUrl() {
            if (window.location.hostname == 'localhost')
                return 'http://localhost:3000'
            else {
                const codespace_host = window.location.hostname.replace('5173', '3000')
                return `https://${codespace_host}`
            }
        }
    },
    methods: {
        addPost() {
            axios.get(`${this.baseUrl}/addPost`, {
                params: {
                    subject: this.subject,
                    entry: this.entry,
                    mood: this.mood
                }
            })
            .then(response => {
                this.outputMsg = response.data.message ;
                
            })
            .catch(error => {
                console.log(error);
            })
        }
    }
}
</script>

<template>
    <div class="table m-2">
        <h3>Add a New Blog Post</h3>

        Subject: <input type="text" size="30" v-model="subject" required>
        <br><br>

        Entry: <br>
        <textarea name="entry" cols="80" rows="5" v-model="entry" required></textarea>
        <br><br>

        Mood:
        <select v-model="mood" required>
            <option disabled value="">Select mood</option>
            <option v-for="m in moods" :key="m" :value="m">{{ m }}</option>
        </select>

        <br>
        <button @click="addPost">Submit New Post</button>
        <br><br>
        {{ outputMsg }}
        <hr>
        Click <router-link to="/ViewPosts/">here</router-link> to return to Main Page
    </div>
</template>

<style scoped>
.table {
    font-family: Arial, sans-serif;
}
textarea, input, select {
    font-family: monospace;
}
button {
    margin-top: 10px;
}
</style>
