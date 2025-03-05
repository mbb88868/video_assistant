<script lang="ts" setup>
defineOptions({
  // 命名当前组件
  name: "ElementPlus"
});
const form = ref({
  videoFile: "",
  title: "",
  tags: "",
  location: ""
});
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
          <el-form label-position="top" :model="form" label-width="80px">
            <div class="content-introduce">
              <div>
                <el-button type="primary" class="long-btn" style="margin-right: 12px;">
                  添加本地视频
                </el-button>
                <el-button type="primary" class="long-btn" plain>
                  已添加的视频
                </el-button>
              </div>
              <span
                style="font-size: 12px; color: #9f9f9f;margin: 12px 0 20px 0;"
              >本地视频支持MP4、MOV、MKV、AVI、FLV、MPEG、OGG、VOB、WEBM、WMV、RMVB等格式，最大4G，较大视频请压缩后上传。</span>
            </div>

            <el-form-item label="标题">
              <el-input v-model="form.title" placeholder="请输入标题" />
            </el-form-item>
            <el-form-item label="视频简介">
              <el-input v-model="form.tags" placeholder="请输入视频简介" />
            </el-form-item>
            <el-form-item label="标签">
              <el-input v-model="form.location" placeholder="请输入标签" />
            </el-form-item>
          </el-form>
        </el-card>
      </div>
      <div class="right-platform">
        <!-- 平台组件 -->
        <div class="pingtai">
          <span class="pingtai-item">平台</span>
          <span class="pingtai-item">抖音1</span>
          <span class="pingtai-item">抖音2</span>
          <span class="pingtai-item">抖音3</span>
          <span class="pingtai-item">抖音4</span>
        </div>
        <el-card class="pingtai-conter">
          <el-form label-position="top" :model="form" label-width="80px">
            <el-form-item label="标题">
              <el-input v-model="form.title" placeholder="请输入标题" />
            </el-form-item>
            <el-form-item label="视频简介">
              <el-input v-model="form.tags" placeholder="请输入视频简介" />
            </el-form-item>
            <el-form-item label="标签">
              <el-input v-model="form.location" placeholder="请输入标签" />
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

    .content-introduce {
      display: flex;
      flex-direction: column;

      .long-btn {
        margin: 0;
        width: 100px;
      }
    }
  }

  .right-platform {
    flex: 1;
    display: flex;

    .pingtai {
      display: flex;
      flex-direction: column;

      .pingtai-item {
        width: 60px;
        height: 60px;
        line-height: 60px;
        text-align: center;
        background-color: #ccc;
        border: 1px solid #fff;
        border-top: 0;
      }
    }

    .pingtai-conter {
      flex: 1;
    }
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
