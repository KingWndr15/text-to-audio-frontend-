<template>
    <div class="container">
        <h1>Text to Speech</h1>
        <div class="input-group">
            <input v-model="text" class="text-input" placeholder="Enter text" />
            <select v-model="language" class="language-select">
                <option value="en">English</option>
                <option value="es">Spanish</option>
                <!-- Add more languages if needed -->
            </select>
        </div>
        <button class="generate-button" @click="generateSpeech">Convert to Speech</button>
        <audio v-if="audioUrl" :src="audioUrl" controls class="audio-player"></audio>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            text: '',
            language: 'en',
            audioUrl: null,
        };
    },
    methods: {
        async generateSpeech() {
            try {
                const response = await axios.post('http://localhost:8000/speech/', {
                    text: this.text,
                    language: this.language,
                }, {
                    responseType: 'blob' // Important for handling binary data
                });

                // Create a URL for the audio file
                this.audioUrl = URL.createObjectURL(response.data);
            } catch (error) {
                console.error('Error generating speech:', error);
            }
        }
    }
}
</script>

<style scoped>
.container {
    max-width: 600px;
    margin: 0 auto;
    padding: 20px;
    background-color: #f9f9f9;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    text-align: center;
}

h1 {
    margin-bottom: 20px;
    color: #333;
}

.input-group {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
}

.text-input {
    width: 65%;
    padding: 10px;
    font-size: 16px;
    border: 1px solid #ccc;
    border-radius: 5px;
}

.language-select {
    width: 30%;
    padding: 10px;
    font-size: 16px;
    border: 1px solid #ccc;
    border-radius: 5px;
}

.generate-button {
    width: 100%;
    padding: 10px;
    font-size: 18px;
    color: #fff;
    background-color: #007bff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
}

.generate-button:hover {
    background-color: #0056b3;
}

.audio-player {
    margin-top: 20px;
    width: 100%;
}
</style>