<template>
  <div>
    <div class="upload-container">
      <input
        type="file"
        ref="fileInput"
        multiple
        @change="handleFileUpload"
        style="display: none"
      />
      <div class="drag-drop-area" @dragover.prevent @drop="handleFileDrop">
        <p>Drag and drop images here</p>
        <p>or</p>
        <button class="upload-button" @click="openFileInput">
          Browse Files
        </button>
      </div>
    </div>
    <div class="preview-container">
      <div
        class="image-preview"
        v-for="(image, index) in previewImages"
        :key="index"
      >
        <img :src="image" alt="Preview" height="50" />
        <button class="remove-button" @click="removePreviewImage(index)">
          Delete
        </button>
      </div>
    </div>
    <!-- <button class="upload-button" @click="uploadImages">Upload</button> -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedFiles: [],
      previewImages: [],
    };
  },
  methods: {
    handleFileUpload(event) {
      const files = event.target.files;
      this.previewImages = [];
      for (let i = 0; i < files.length; i++) {
        const reader = new FileReader();
        reader.onload = (e) => {
          this.previewImages.push(e.target.result);
        };
        reader.readAsDataURL(files[i]);
      }
      this.selectedFiles = Array.from(files);
    },
    handleFileDrop(event) {
      event.preventDefault();
      const files = event.dataTransfer.files;
      this.handleFileUpload({ target: { files } });
    },
    openFileInput() {
      this.$refs.fileInput.click();
    },
    removePreviewImage(index) {
      this.previewImages.splice(index, 1);
      this.selectedFiles.splice(index, 1);
    },
    uploadImages() {
      // Perform the upload logic here using your preferred method (e.g., Axios)
      // Access the selected files using 'this.selectedFiles'
      // Clear the preview and selected files after successful upload
      this.previewImages = [];
      this.selectedFiles = [];
    },
  },
};
</script>

<style>
.upload-container {
  text-align: center;
  margin-bottom: 20px;
  width: 1000px;
}
.drag-drop-area {
  border: 2px dashed #ccc;
  padding: 80px;
  cursor: pointer;
  height: 250px;
}
.preview-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  margin-bottom: 20px;
}
.image-preview {
  position: relative;
  margin: 10px;
  box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px;
  margin-left: 20px;
}
.remove-button {
  position: absolute;
  top: -26px;
  right: 5px;
  padding: 5px;
  background: red;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
.upload-button {
  padding: 10px 20px;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
</style>
