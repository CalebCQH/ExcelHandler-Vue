<template>
  <div>
    <h1>量一 — 打卡文件生成器</h1>
    <input type="file" accept=".xls,.xlsx" @change="handleUpload" />
    <p>状态: {{ status }}</p>
    <a v-if="downloadUrl" :href="downloadUrl.url" :download="downloadUrl.name">下载文件</a>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const fileName = ref(null)
const status = ref('待上传')
const downloadUrl = ref(null)
const processing = ref(false)

async function handleUpload(event) {
  const file = event.target.files[0]
  if (!file) return
  fileName.value = file.name
  status.value = '准备上传'
  downloadUrl.value = null

  const form = new FormData()
  form.append('file', file)

  try {
    processing.value = true
    status.value = '上传中...'
    const res = await fetch('/process', {
      method: 'POST',
      body: form
    })
    if (!res.ok) throw new Error('服务器返回错误')

    const blob = await res.blob()
    const outName = res.headers.get('X-Output-Filename') || ('output_' + file.name)
    const url = URL.createObjectURL(blob)
    downloadUrl.value = { url, name: outName }
    status.value = '处理完成，点击下载'
  } catch (err) {
    status.value = '出错：' + (err.message || '未知错误')
  } finally {
    processing.value = false
  }
}
</script>