<template>
  <div class="cards-container">
    <!-- 量一打卡卡片 -->
    <div class="upload-card">
      <div class="header">
        <h1 class="title">量一打卡</h1>
        <p class="subtitle">上传Excel文件生成考勤报表</p>
      </div>

      <div class="upload-area"
           @click="triggerFileInput('liangyi')"
           @dragover.prevent="handleDragOver('liangyi')"
           @dragleave="handleDragLeave('liangyi')"
           @drop.prevent="handleDrop($event, 'liangyi')"
           :class="{ 'drag-active': isDragging.liangyi }">
        <div class="upload-icon">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"></path>
            <polyline points="17 8 12 3 7 8"></polyline>
            <line x1="12" y1="3" x2="12" y2="15"></line>
          </svg>
        </div>
        <p class="upload-text">拖放文件到此处或<span class="highlight">浏览文件</span></p>
        <p class="upload-hint">支持 .xlsx, .xls 格式 (最大20MB)</p>
        <input
            type="file"
            ref="fileInputLiangyi"
            @change="handleFileUpload($event, 'liangyi')"
            class="file-input"
            accept=".xlsx, .xls"
        />
      </div>

      <div v-if="selectedFile.liangyi" class="file-info">
        <div class="file-icon">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
            <polyline points="14 2 14 8 20 8"></polyline>
          </svg>
        </div>
        <div class="file-details">
          <p class="file-name">{{ selectedFile.liangyi.name }}</p>
          <p class="file-size">{{ formatFileSize(selectedFile.liangyi.size) }} · 等待上传</p>
        </div>
        <button @click="clearFile('liangyi')" class="clear-btn" title="移除文件">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <line x1="18" y1="6" x2="6" y2="18"></line>
            <line x1="6" y1="6" x2="18" y2="18"></line>
          </svg>
        </button>
      </div>

      <div class="action-container">
        <button
            @click="uploadFile('liangyi')"
            :disabled="!selectedFile.liangyi || loading.liangyi"
            class="action-btn"
            :class="{ 'loading': loading.liangyi }"
        >
          <span v-if="loading.liangyi" class="loading-spinner"></span>
          {{ loading.liangyi ? '正在生成考勤文件...' : '开始处理' }}
        </button>

        <p class="security-note">
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="#3498db" viewBox="0 0 24 24">
            <path d="M12 1L3 5v6c0 5.55 3.84 10.74 9 12 5.16-1.26 9-6.45 9-12V5l-9-4zm0 10.99h7c-.53 4.12-3.28 7.79-7 8.94V12H5V6.3l7-3.11v8.8z"/>
          </svg>
          文件仅用于考勤处理，不会存储到服务器
        </p>
      </div>

      <transition name="fade">
        <div v-if="errorMessage.liangyi" class="status-message error">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <circle cx="12" cy="12" r="10"></circle>
            <line x1="12" y1="8" x2="12" y2="12"></line>
            <line x1="12" y1="16" x2="12.01" y2="16"></line>
          </svg>
          <span>{{ errorMessage.liangyi }}</span>
        </div>
      </transition>
    </div>

    <!-- 容维打卡卡片 -->
    <div class="upload-card">
      <div class="header">
        <h1 class="title">容维打卡</h1>
        <p class="subtitle">上传Excel文件生成考勤报表</p>
      </div>

      <div class="upload-area"
           @click="triggerFileInput('rongwei')"
           @dragover.prevent="handleDragOver('rongwei')"
           @dragleave="handleDragLeave('rongwei')"
           @drop.prevent="handleDrop($event, 'rongwei')"
           :class="{ 'drag-active': isDragging.rongwei }">
        <div class="upload-icon">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M21 15v4a2 极 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"></path>
            <polyline points="17 8 12 3 7 8"></polyline>
            <line x1="12" y1="3" x2="12" y2="15"></line>
          </svg>
        </div>
        <p class="upload-text">拖放文件到此处或<span class="highlight">浏览文件</span></p>
        <p class="upload-hint">支持 .xlsx, .xls 格式 (最大20MB)</p>
        <input
            type="file"
            ref="fileInputRongwei"
            @change="handleFileUpload($event, 'rongwei')"
            class="file-input"
            accept=".xlsx, .xls"
        />
      </div>

      <div v-if="selectedFile.rongwei" class="file-info">
        <div class="file-icon">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
            <polyline points="14 2 14 8 20 8"></polyline>
          </svg>
        </div>
        <div class="file-details">
          <p class="file-name">{{ selectedFile.rongwei.name }}</p>
          <p class="file-size">{{ formatFileSize(selectedFile.rongwei.size) }} · 等待上传</p>
        </div>
        <button @click="clearFile('rongwei')" class="clear-btn" title="移除文件">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <line x1="18" y1="6" x2="6" y2="18"></line>
            <line x1="6" y1="6" x2="18" y2="18"></line>
          </svg>
        </button>
      </div>

      <div class="action-container">
        <button
            @click="uploadFile('rongwei')"
            :disabled="!selectedFile.rongwei || loading.rongwei"
            class="action-btn"
            :class="{ 'loading': loading.rongwei }"
        >
          <span v-if="loading.rongwei" class="loading-spinner"></span>
          {{ loading.rongwei ? '正在生成考勤文件...' : '开始处理' }}
        </button>

        <p class="security-note">
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="#3498db" viewBox="0 0 24 24">
            <path d="M12 1L3 5v6c0 5.55 3.84 10.74 9 12 5.16-1.26 9-6.45 9-12V5l-9-4zm0 10.99h7c-.53 4.12-3.28 7.79-7 8.94V12H5V6.3l7-3.11v8.8z"/>
          </svg>
          文件仅用于考勤处理，不会存储到服务器
        </p>
      </div>

      <transition name="fade">
        <div v-if="errorMessage.rongwei" class="status-message error">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <circle cx="12" cy="12" r="10"></circle>
            <line x1="12" y1="8" x2="12" y2="12"></line>
            <line x1="12" y1="16" x2="12.01" y2="16"></line>
          </svg>
          <span>{{ errorMessage.rongwei }}</span>
        </div>
      </transition>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

// 为两个卡片分别创建独立的状态
const selectedFile = ref({
  liangyi: null,
  rongwei: null
})

const loading = ref({
  liangyi: false,
  rongwei: false
})

const errorMessage = ref({
  liangyi: '',
  rongwei: ''
})

const isDragging = ref({
  liangyi: false,
  rongwei: false
})

const fileInputLiangyi = ref(null)
const fileInputRongwei = ref(null)

function triggerFileInput(type) {
  if (type === 'liangyi') {
    fileInputLiangyi.value.click()
  } else if (type === 'rongwei') {
    fileInputRongwei.value.click()
  }
}

function handleFileUpload(event, type) {
  const file = event.target.files[0]
  if (validateFile(file, type)) {
    selectedFile.value[type] = file
    errorMessage.value[type] = ''
  }
}

function validateFile(file, type) {
  const validTypes = ['.xlsx', '.xls']
  const fileExtension = file.name.split('.').pop().toLowerCase()

  if (!validTypes.includes('.' + fileExtension)) {
    errorMessage.value[type] = '仅支持Excel文件 (.xlsx, .xls)'
    return false
  }

  if (file.size > 20 * 1024 * 1024) {
    errorMessage.value[type] = '文件大小不能超过20MB'
    return false
  }

  return true
}

function handleDragOver(type) {
  isDragging.value[type] = true
}

function handleDragLeave(type) {
  isDragging.value[type] = false
}

function handleDrop(e, type) {
  isDragging.value[type] = false
  const file = e.dataTransfer.files[0]
  if (file && validateFile(file, type)) {
    selectedFile.value[type] = file
  }
}

function clearFile(type) {
  selectedFile.value[type] = null
  if (type === 'liangyi' && fileInputLiangyi.value) {
    fileInputLiangyi.value.value = ''
  } else if (type === 'rongwei' && fileInputRongwei.value) {
    fileInputRongwei.value.value = ''
  }
}

function formatFileSize(bytes) {
  if (bytes === 0) return '0 Bytes'
  const k = 1024
  const sizes = ['Bytes', 'KB', 'MB', 'GB']
  const i = Math.floor(Math.log(bytes) / Math.log(k))
  return parseFloat((bytes / Math.pow(k, i)).toFixed(1)) + ' ' + sizes[i]
}

async function uploadFile(type) {
  if (!selectedFile.value[type]) return

  loading.value[type] = true
  errorMessage.value[type] = ''

  const formData = new FormData()
  formData.append('file', selectedFile.value[type])

  try {
    // 根据卡片类型使用不同的API端点
    const endpoint = type === 'liangyi'
        ? 'http://127.0.0.1:5000/process'
        : 'http://127.0.0.1:5000/process-rongwei'

    const res = await fetch(endpoint, {
      method: 'POST',
      body: formData,
      mode: 'cors',
      headers: {
        'Accept': 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet'
      }
    })

    if (!res.ok) {
      const errorData = await res.json()
      throw new Error(errorData.message || '处理失败，请检查文件格式')
    }

    const blob = await res.blob()
    const url = window.URL.createObjectURL(blob)
    const a = document.createElement('a')
    a.href = url

    // 根据卡片类型设置不同的下载文件名
    const prefix = type === 'liangyi' ? '量一考勤报表' : '容维考勤报表'
    a.download = prefix + '_' + new Date().toISOString().slice(0, 10) + '.xlsx'

    document.body.appendChild(a)
    a.click()
    a.remove()
    window.URL.revokeObjectURL(url)
  } catch (err) {
    errorMessage.value[type] = err.message || '请求失败: ' + err.message
  } finally {
    loading.value[type] = false
  }
}

// 在组件挂载时添加全局样式
onMounted(() => {
  const style = document.createElement('style')
  style.textContent = `
    html, body {
      margin: 0;
      padding: 0;
      height: 100%;
      overflow: hidden;
      background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }

    #app {
      height: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
    }
  `;
  document.head.appendChild(style);
});
</script>

<style scoped>
/* 卡片容器布局 */
.cards-container {
  display: flex;
  justify-content: center;
  gap: 40px;
  width: 100%;
  max-width: 1400px;
  margin: 0 auto;
  padding: 20px;
}

/* 卡片样式 */
.upload-card {
  background-color: white;
  border-radius: 20px;
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.25);
  padding: 50px;
  width: 100%;
  max-width: 600px;
  text-align: center;
  position: relative;
  overflow: hidden;
  flex: 1;
}

.upload-card::before {
  content: "";
  position: absolute;
  top: -50px;
  right: -50px;
  width: 150px;
  height: 150px;
  background: linear-gradient(45deg, #3498db, #2c3e50);
  border-radius: 50%;
  opacity: 0.1;
}

/* 标题区域 */
.header {
  margin-bottom: 40px;
  position: relative;
}

.title {
  font-size: 2.5rem;
  font-weight: 800;
  margin: 0 0 10px;
  background: linear-gradient(to right, #2c3e50, #3498db);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
  letter-spacing: -0.5px;
}

.subtitle {
  font-size: 1.2rem;
  color: #7f8c8d;
  margin-top: 0;
  font-weight: 400;
}

/* 上传区域 */
.upload-area {
  border: 2px dashed #3498db;
  border-radius: 16px;
  padding: 60px 40px;
  background-color: rgba(52, 152, 219, 0.05);
  cursor: pointer;
  transition: all 0.3s ease;
  margin-bottom: 30px;
  position: relative;
}

.upload-area.drag-active {
  background-color: rgba(52, 152, 219, 0.15);
  border-color: #1a73e8;
  transform: translateY(-3px);
}

.upload-area:hover {
  background-color: rgba(52, 152, 219, 0.1);
  border-color: #2980b9;
}

.upload-icon {
  width: 70px;
  height: 70px;
  margin: 0 auto 20px;
  color: #3498db;
  transition: transform 0.3s;
}

.upload-area:hover .upload-icon {
  transform: translateY(-5px);
}

.upload-text {
  font-size: 1.3rem;
  font-weight: 600;
  color: #2c3e50;
  margin: 0 0 12px;
}

.upload-text .highlight {
  color: #3498db;
  text-decoration: underline;
  font-weight: 700;
}

.upload-hint {
  font-size: 1rem;
  color: #7f8c8d;
  margin: 0;
  font-weight: 500;
}

.file-input {
  display: none;
}

/* 文件信息区域 */
.file-info {
  display: flex;
  align-items: center;
  background-color: #f8f9fa;
  border-radius: 12px;
  padding: 18px 20px;
  margin-bottom: 30px;
  border: 1px solid #e9ecef;
  transition: all 0.3s;
}

.file-info:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  transform: translateY(-2px);
}

.file-icon {
  width: 44px;
  height: 44px;
  color: #3498db;
  margin-right: 18px;
}

.file-details {
  flex: 1;
  text-align: left;
}

.file-name {
  font-size: 1.1rem;
  font-weight: 500;
  color: #2c3e50;
  margin: 0 0 6px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.file-size {
  font-size: 0.9rem;
  color: #7f8c8d;
  margin: 0;
  font-weight: 500;
}

.clear-btn {
  background: none;
  border: none;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  color: #e74c3c;
  transition: background-color 0.2s;
}

.clear-btn:hover {
  background-color: rgba(231, 76, 60, 0.1);
}

/* 操作区域 */
.action-container {
  margin-top: 30px;
}

.action-btn {
  width: 100%;
  padding: 18px;
  font-size: 1.2rem;
  font-weight: 700;
  border: none;
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.3s ease;
  background: linear-gradient(to right, #3498db, #2980b9);
  color: white;
  position: relative;
  overflow: hidden;
  box-shadow: 0 6极 20px rgba(52, 152, 219, 0.4);
  letter-spacing: 0.5px;
}

.action-btn:hover:not(:disabled) {
  transform: translateY(-4px);
  box-shadow: 0 8px 25px rgba(52, 152, 219, 0.5);
}

.action-btn:disabled {
  background: #bdc3c7;
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
}

.security-note {
  font-size: 0.9rem;
  color: #7f8c8d;
  margin-top: 15px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

/* 加载动画 */
.loading-spinner {
  display: inline-block;
  width: 22px;
  height: 22px;
  border: 3px solid rgba(255, 255, 255, 0.3);
  border-top: 3px solid white;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin-right: 12px;
  vertical-align: middle;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

/* 状态消息 */
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}

.status-message {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 16px;
  border-radius: 10px;
  margin-top: 25px;
  font-size: 1rem;
  animation: slideIn 0.4s ease;
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.status-message svg {
  width: 24px;
  height: 24px;
  margin-right: 12px;
}

.status-message.error {
  background-color: rgba(231, 76, 60, 0.12);
  color: #e74c3c;
  border: 1px solid rgba(231, 76, 60, 0.2);
}
</style>