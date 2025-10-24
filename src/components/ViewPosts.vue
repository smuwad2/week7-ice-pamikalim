<script setup>
import axios from 'axios'
</script>

<script>
export default {
    data() {
        return {
            moods: ["Happy", "Sad", "Angry"],
            posts: [], // all posts from backend
            entry: "", // entry being edited
            mood: "",  // mood being edited
            showEditPost: false, // show/hide edit form
            editPostId: "" // which post is being edited
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
    created() {
        // fetch all posts when page loads
        axios.get(`${this.baseUrl}/posts`)
            .then(response => {
                this.posts = response.data
            })
            .catch(error => {
                this.posts = [{ entry: 'Error: ' + error.message }]
            })
    },
    methods: {
        editPost(id) {
            const post = this.posts.find(p => p.id === id)
            if (post) {
                this.editPostId = id
                this.entry = post.entry
                this.mood = post.mood
                this.showEditPost = true
            }
        },

        updatePost(event) {
            event.preventDefault() // prevent page reload

            axios.post(`${this.baseUrl}/updatePost`, {
                id: this.editPostId,
                entry: this.entry,
                mood: this.mood
            })
            .then(response => {
                alert('Post updated successfully!')

                // update the post in the local posts array
                const index = this.posts.findIndex(p => p.id === this.editPostId)
                if (index !== -1) {
                    this.posts[index].entry = this.entry
                    this.posts[index].mood = this.mood
                }

                // reset form
                this.showEditPost = false
                this.editPostId = ""
                this.entry = ""
                this.mood = ""
            })
            .catch(error => {
                console.error('Update failed:', error)
                alert('Error updating post.')
            })
        }
    }
}
</script>

<template>
    <div id="demo">
        <h2>View Blog Posts</h2>

        <table class="table m-2">
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Entry</th>
                    <th>Mood</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="post in posts" :key="post.id">
                    <td>{{ post.id }}</td>
                    <td>{{ post.entry }}</td>
                    <td>{{ post.mood }}</td>
                    <td>
                        <button @click="editPost(post.id)">Edit</button>
                    </td>
                </tr>
            </tbody>
        </table>

        <div id="editPost" v-if="showEditPost">
            <h3>Edit Post (ID: {{ editPostId }})</h3>
            <div id="postContent" class="mx-3">
                <form @submit="updatePost">
                    <div class="mb-3">
                        <label for="entry" class="form-label">Entry</label>
                        <textarea id="entry" class="form-control" v-model="entry" required></textarea>
                    </div>

                    <div class="mb-3">
                        <label for="mood" class="form-label">Mood</label>
                        <select id="mood" class="form-select" v-model="mood" required>
                            <option value="" disabled>Select Mood</option>
                            <option v-for="m in moods" :key="m" :value="m">{{ m }}</option>
                        </select>
                    </div>

                    <button type="submit" class="btn btn-primary">Update Post</button>
                </form>
            </div>
        </div>
    </div>
</template>

<style scoped>
table {
    width: 100%;
    border-collapse: collapse;
}
th, td {
    border: 1px solid #aaa;
    padding: 6px;
}
button {
    padding: 4px 8px;
    cursor: pointer;
}
</style>
