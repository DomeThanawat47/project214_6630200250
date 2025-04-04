<template>
    <div class="image-gallery">
      <div class="image-wrapper" v-for="(img, index) in images" :key="index">
        <img :src="img" alt="Student Image">
      </div>
      <div class="upload-container">
        <input type="file" accept="image/*" @change="handleImageUpload" ref="fileInput">
        <button class="action-btn" @click="$refs.fileInput.click()">เพิ่มรูปภาพ</button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      images: {
        type: Array,
        default: () => []
      }
    },
    methods: {
      handleImageUpload(event) {
        const file = event.target.files[0]
        if (file) {
          const imageUrl = URL.createObjectURL(file)
          this.$emit('add-image', imageUrl)
        }
      }
    }
  }
  </script>
  
  <style scoped>
  .image-gallery {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); /* ปรับขนาดขั้นต่ำให้ใหญ่ขึ้น */
    gap: 20px; /* เพิ่มระยะห่างระหว่างรูป */
    margin-bottom: 30px;
  }
  
  .image-wrapper {
    position: relative;
    transition: all 0.3s ease;
  }
  
  .image-wrapper img {
    width: 150px; /* ปรับขนาดรูปภาพให้ใหญ่ขึ้น */
    height: 150px;
    object-fit: cover;
    border: 2px solid #00ff00;
    border-radius: 10px;
    transition: all 0.3s ease;
  }
  
  .image-wrapper:hover img {
    transform: scale(1.1);
    box-shadow: 0 0 20px rgba(0, 255, 0, 0.8);
  }
  
  .upload-container {
    display: flex;
    justify-content: center;
    margin-top: 20px;
  }
  
  .upload-container input[type="file"] {
    display: none;
  }
  
  .action-btn {
    padding: 8px 16px;
    background: transparent;
    color: #00ff00;
    border: 2px solid #00ff00;
    border-radius: 5px;
    cursor: pointer;
    font-family: 'JetBrains Mono', 'Fira Code', 'Courier New', monospace;
    font-size: 14px;
    transition: all 0.3s ease;
    box-shadow: 0 0 10px rgba(0, 255, 0, 0.5);
  }
  
  .action-btn:hover {
    background: #00ff00;
    color: #000;
    box-shadow: 0 0 20px rgba(0, 255, 0, 0.8);
  }
  
  @media (max-width: 768px) {
    .image-gallery {
      grid-template-columns: repeat(auto-fit, minmax(120px, 1fr)); /* ปรับขนาดขั้นต่ำสำหรับหน้าจอเล็ก */
    }
  
    .image-wrapper img {
      width: 120px; /* ปรับขนาดรูปภาพให้เล็กลงในหน้าจอเล็ก */
      height: 120px;
    }
  }
  
  @media (max-width: 480px) {
    .image-gallery {
      grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
    }
  
    .image-wrapper img {
      width: 100px;
      height: 100px;
    }
  }
  </style>