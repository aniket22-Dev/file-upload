<template>
    <div class="file-uploader">
        <div class="drop-area" @dragover.prevent @dragenter.prevent @drop.prevent="handleDrop">
            <p v-if="!uploading">Drag file here</p>
            <p v-else>Uploading...</p>
        </div>
        <div v-if="uploading" class="file-uploading">
            <p v-if="fileName">Uploading: {{ fileName }}</p>
            <progress :value="progress" max="100"></progress>
            <p>Progress: {{ progress }}%</p>
        </div>
        <p v-if="uploadSuccess">File uploaded successfully!</p>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const uploading = ref(false);
const progress = ref(0);
const fileName = ref('');
const uploadSuccess = ref(false);

const handleDrop = async (event: DragEvent) => {
    event.preventDefault();
    const file = event.dataTransfer?.files[0];

    if (file) {
        fileName.value = file.name; // Store the uploaded file name
        await uploadFile(file);
    }
};

const uploadFile = async (file: File) => {
    uploading.value = true;

    // Simulated progress: 10%
    setTimeout(() => {
        progress.value = 10;
    }, 1000);

    // Simulated progress: 40%
    setTimeout(() => {
        progress.value = 40;
    }, 3000);

    // Simulated progress: 100% and successful completion
    setTimeout(() => {
        progress.value = 100;
        uploading.value = false;
        uploadSuccess.value = true; // Set uploadSuccess to true on successful completion
    }, 5000);
};

// function to update the progress bar during the upload
const updateProgress = (event: ProgressEvent<XMLHttpRequestEventTarget>) => {
    progress.value = Math.round((event.loaded / event.total) * 100);
};
</script>

<style>
.file-uploader {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    width: 50%;
    margin: 0 auto;
    flex-direction: column;
}

.drop-area {
    border: 2px dashed #ccc;
    padding: 20px;
    text-align: center;
    cursor: pointer;
    width: 100%;
}

progress {
    width: 100%;
    height: 20px;
}

.file-uploading {
    justify-content: center;
    align-items: center;
    width: 100%;
    margin: 0 auto;
}
</style>