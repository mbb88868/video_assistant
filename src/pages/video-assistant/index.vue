<script lang="ts" setup>
defineOptions({
  // 命名当前组件
  name: "ElementPlus"
})
const form = ref({
  videoFile: "",
  title: "",
  tags: "",
  location: ""
})
function submitForm() {
  consle.log(123)
}
</script>

<template>
  <div class="app-container">
    <div class="video-assistant">
      <div class="left-fun-btn">
        <el-button class="btn-item">
          按钮funxxxxx
        </el-button>
        <el-button class="btn-item">
          按钮funxxxxx
        </el-button>
        <el-button class="btn-item">
          按钮funxxxxx
        </el-button>
        <el-button class="btn-item">
          按钮funxxxxx
        </el-button>
        <el-button class="btn-item">
          按钮funxxxxx
        </el-button>
        <el-button class="btn-item">
          按钮funxxxxx
        </el-button>
      </div>
      <div class="middle-content">
        <!-- 视频组件 -->
        <el-card>
          <el-form :model="form" label-width="80px">
            <el-form-item label="视频文件">
              <el-input v-model="form.videoFile" placeholder="请选择视频文件" />
            </el-form-item>
            <el-form-item label="标题">
              <el-input v-model="form.title" placeholder="请输入标题" />
            </el-form-item>
            <el-form-item label="标签">
              <el-input v-model="form.tags" placeholder="请输入标签" />
            </el-form-item>
            <el-form-item label="位置">
              <el-input v-model="form.location" placeholder="请输入位置" />
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="submitForm">
                发布
              </el-button>
            </el-form-item>
          </el-form>
        </el-card>
      </div>
      <div class="right-platform">
        <!-- 平台组件 -->
        <el-card>
          <el-form :model="form" label-width="80px">
            <el-form-item label="视频文件">
              <el-input v-model="form.videoFile" placeholder="请选择视频文件" />
            </el-form-item>
            <el-form-item label="标题">
              <el-input v-model="form.title" placeholder="请输入标题" />
            </el-form-item>
            <el-form-item label="标签">
              <el-input v-model="form.tags" placeholder="请输入标签" />
            </el-form-item>
            <el-form-item label="位置">
              <el-input v-model="form.location" placeholder="请输入位置" />
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="submitForm">
                发布
              </el-button>
            </el-form-item>
          </el-form>
        </el-card>
      </div>
    </div>
    <div class="fixed-bottom-div">
      <el-button>保存</el-button>
      <el-button>同步至草稿</el-button>
      <el-button>发布</el-button>
      <el-button>定时发布</el-button>
      <el-button>关闭</el-button>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.video-assistant {
  display: flex;

  .left-fun-btn {
    display: flex;
    flex-direction: column;
    margin-right: 20px;

    .btn-item {
      margin: 0;
      margin-bottom: 12px;
    }
  }

  .middle-content {
    flex: 1;
  }
}

.fixed-bottom-div {
  position: fixed;
  bottom: 0px;
  /* 调整按钮距离底部的距离 */
  left: 50%;
  /* 水平居中 */
  transform: translateX(-50%);
  /* 水平居中 */
  display: flex;
  justify-content: center;
  background: #fff;
  width: 100%;
  padding: 20px 0;

  /* 按钮之间的间距 */
  .button {
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
  }
}
</style>
