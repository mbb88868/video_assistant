<script lang="ts" setup>
import { defineEmits, ref } from "vue";

// 文件列表

// 定义事件，用于将文件信息传递给父组件
const emit = defineEmits<{
  (e: "files-uploaded", files: { name: string; size: number; url: string }[]): void;
}>(); // 本地状态
const fileInput = ref<HTMLInputElement | null>(null); // 文件选择框的引用
const fileList = ref<{ name: string; size: number; url: string }[]>([]);// 触发文件选择框
function triggerFileSelect() {
  if (fileInput.value) {
    fileInput.value.click(); // 手动触发文件选择框
  }
}

// 检查文件是否已经存在
function isFileExist(name: string, size: number): boolean {
  return fileList.value.some(file => file.name === name && file.size === size);
}

// 文件选择变化时触发
function handleFileSelect(event: Event) {
  const target = event.target as HTMLInputElement;
  if (target.files) {
    const files = target.files;

    // 遍历文件并检查是否重复
    for (let i = 0; i < files.length; i++) {
      const file = files[i];

      // 校验文件类型
      if (!file.type.startsWith("video/")) {
        // eslint-disable-next-line no-alert
        alert(`文件 ${file.name} 不是视频文件！`);
        continue;
      }

      // 校验文件大小（限制为 4GB）
      const isLt4GB = file.size <= 4 * 1024 * 1024 * 1024;
      if (!isLt4GB) {
        // eslint-disable-next-line no-alert
        alert(`文件 ${file.name} 大小超过 4GB！`);
        continue;
      }

      // 检查文件是否已经存在
      if (isFileExist(file.name, file.size)) {
        console.log(`文件 ${file.name} 已经存在，跳过上传。`);
        continue;
      }

      // 生成临时 URL 并添加到文件列表
      const fileUrl = URL.createObjectURL(file);
      fileList.value.push({
        name: file.name,
        size: file.size,
        url: fileUrl
      });

      // 触发事件，将文件信息传递给父组件
      // eslint-disable-next-line vue/custom-event-name-casing
      emit("files-uploaded", fileList.value);

      console.log(`文件 ${file.name} 已添加到列表。`);
    }

    console.log("文件列表:", fileList.value);
  }
}

// 删除文件
function deleteFile(index: number) {
  const deletedFile = fileList.value[index];

  // 释放临时 URL
  if (deletedFile.url) {
    URL.revokeObjectURL(deletedFile.url);
  }

  // 从文件列表中移除文件
  fileList.value.splice(index, 1);

  console.log(`文件 ${deletedFile.name} 已删除。`);
}

// 清除缓存
function clearCache() {
  // 释放所有临时 URL
  fileList.value.forEach((file) => {
    if (file.url) {
      URL.revokeObjectURL(file.url);
    }
  });
  fileList.value = [];
  console.log("缓存已清除，文件列表为空。");
}
</script>

<template>
  <div class="unique-file-uploader">
    <!-- 文件上传按钮 -->
    <input
      type="file"
      accept="video/*"
      ref="fileInput"
      style="display: none;"
      multiple
      @change="handleFileSelect"
    >
    <el-button type="primary" @click="triggerFileSelect">
      上传视频
    </el-button>

    <!-- 清除缓存按钮 -->
    <el-button type="warning" @click="clearCache">
      清除缓存
    </el-button>

    <!-- 文件列表 -->
    <div v-if="fileList.length > 0" class="file-list">
      <h4>已上传的文件：</h4>
      <ul style="margin: 0; padding: 0;">
        <li v-for="(file, index) in fileList" :key="index" class="file-item">
          <!-- 文件信息 -->

          <el-tooltip :content="file.name" placement="top">
            <p style="width: 30%;display: inline-block; margin: 0; overflow: hidden; text-overflow: ellipsis; white-space: nowrap;">
              <strong>文件名：</strong>{{ file.name }}
            </p>
          </el-tooltip>

          <el-tooltip :content="`${(file.size / 1024 / 1024).toFixed(2)} MB`" placement="top">
            <p style="width: 15%;display: inline-block; margin: 0; overflow: hidden; text-overflow: ellipsis; white-space: nowrap;">
              <strong>大小：</strong>{{ (file.size / 1024 / 1024).toFixed(2) }} MB
            </p>
          </el-tooltip>

          <el-tooltip :content="file.url" placement="top">
            <p style="width: 55%;display: inline-block; margin: 0; overflow: hidden; text-overflow: ellipsis; white-space: nowrap;">
              <strong>临时 URL:</strong>{{ file.url }}
            </p>
          </el-tooltip>
          <!-- 删除按钮 -->
          <el-icon :size="20" color="#F56C6C" style=" cursor: pointer;">
            <delete @click="deleteFile(index)" />
          </el-icon>
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.unique-file-uploader {
  padding: 20px;
}

.file-list {
  margin-top: 20px;
}

.file-item {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
  padding: 10px;
  border: 1px solid #ebeef5;
  border-radius: 4px;
}

.file-item .el-button {
  margin-right: 10px;
}

.file-item p {
  font-size: 14px;
  color: #606266;
  margin: 0;
}
</style>
