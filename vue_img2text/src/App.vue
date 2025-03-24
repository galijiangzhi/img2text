<script setup>
import { ref } from 'vue';

// 定义响应式变量
const selectedImage = ref(null);
const imagePreviewUrl = ref('');
const resultText = ref('');

// 处理图片选择
const handleImageSelect = (event) => {
  const file = event.target.files[0];
  if (file) {
    selectedImage.value = file;
    imagePreviewUrl.value = URL.createObjectURL(file);
  }
};

// 处理图片上传
const handleImageUpload = async () => {
  if (!selectedImage.value) {
    alert('请先选择一张图片');
    return;
  }

  const formData = new FormData();
  formData.append('image', selectedImage.value);

  try {
    const response = await fetch('http://192.168.1.1:8888/upload', {
      method: 'POST',
      body: formData,
    });

    if (!response.ok) {
      throw new Error('上传失败');
    }

    const data = await response.json();
    resultText.value = data.result; // 假设后端返回的结果在 data.result 中
  } catch (error) {
    console.error('上传出错:', error);
    resultText.value = '上传出错，请重试';
  }
};
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo-2021.png" width="125" height="125" />
    <img alt="myhead" class="myhead" src="./assets/image.png" />
    
    <div class="wrapper">
      <HelloWorld msg="You did it!" />
    </div>
  </header>

  <main>
    <TheWelcome />

    <!-- 图片预览框 -->
    <div class="image-preview">
      <img
        v-if="imagePreviewUrl"
        :src="imagePreviewUrl"
        alt="Preview"
        class="preview-image"
      />
      <p v-else>未选择图片</p>
    </div>

    <!-- 图片选择和上传按钮 -->
    <div class="upload-section">
      <input type="file" accept="image/*" @change="handleImageSelect" />
      <button @click="handleImageUpload">上传图片</button>
    </div>

    <!-- 结果显示框 -->
    <div class="result-box">
      <h3>结果:</h3>
      <p>{{ resultText }}</p>
    </div>
  </main>
</template>

<style>

body {
  background-color: #808080;
  color: white; /* 将文字颜色设置为白色，以便在黑色背景上可见 */
}

header {
  line-height: 1.5;
}

.logo {
  position: absolute; /* 设置为绝对定位 */
  top: 20%; /* 距离顶部的距离 */
  left: 17%; /* 距离左侧的距离，这里设置为页面水平居中 */
  transform: translateX(-50%); /* 通过 transform 实现水平居中 */
  margin: 0 auto 2rem;
  width: 320px; /* 限制预览框宽度 */
  height: 200px; /* 限制预览框高度 */
}


.myhead {
  position: absolute; /* 设置为绝对定位 */
  top: 50%; /* 距离顶部的距离 */
  left: 18%; /* 距离左侧的距离，这里设置为页面水平居中 */
  transform: translateX(-50%); /* 通过 transform 实现水平居中 */
  margin: 0 auto 2rem;
  width: 540px; /* 限制预览框宽度 */
  height: 80px; /* 限制预览框高度 */
}
.image-preview {
  margin: 2rem auto;
  text-align: center;
  width: 800px; /* 限制预览框宽度 */
  height: 400px; /* 限制预览框高度 */
  overflow: hidden; /* 超出部分隐藏 */
  display: flex;
  justify-content: center;
  align-items: center;
  border: 1px solid #ccc;
  border-radius: 8px;
  background-color: #f9f9f9;
}

.preview-image {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain; /* 保持图片比例，完整显示 */
}

.upload-section {
  margin: 2rem auto;
  text-align: center;
}

.upload-section input {
  margin-right: 1rem;
}

.result-box {
  margin: 2rem auto;
  padding: 1rem;
  border: 1px solid #ccc;
  border-radius: 8px;
  background-color: #f9f9f9;
  text-align: center;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>