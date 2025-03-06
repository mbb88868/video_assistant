<script lang="ts" setup>
import { CirclePlus } from "@element-plus/icons-vue";
import _ from "lodash";
import UniqueFileUploader from "./component/video-upload-dialog.vue";
// 存储上传的文件信息
const uploadedFiles = ref<{ name: string; size: number }[]>([]);

// 处理文件上传事件
function handleFilesUploaded(files: { name: string; size: number; url: string }[]) {
  // 只存储文件名和大小
  uploadedFiles.value = files.map(file => ({
    name: file.name,
    size: file.size,
    url: file.url
  }));

  console.log("上传的文件信息:", uploadedFiles.value);
}
const form = ref({
  title: "",
  content: "",
  tags: []
});

// 初始化卡片库（修正拼写错误）
const containerLibrary = ref([
  {
    id: 1,
    title: "",
    content: "",
    tags: [],
    status: "edit"
  }
]);

// 添加新卡片
function addCard() {
  const newId = containerLibrary.value.length > 0
    ? Math.max(...containerLibrary.value.map(c => c.id)) + 1
    : 1;
  containerLibrary.value.push({
    id: newId,
    title: "",
    content: "",
    tags: [],
    status: "edit"
  });
}

function saveCard(card: any) {
  const index = containerLibrary.value.findIndex(c => c.id === card.id);
  if (index !== -1) {
    containerLibrary.value[index] = { ...containerLibrary.value[index], ...card };
    containerLibrary.value[index].status = "read";
  }
}

function editCard(card: any) {
  const index = containerLibrary.value.findIndex(c => c.id === card.id);
  if (index !== -1) {
    containerLibrary.value[index].status = "edit";
  }
}

// 删除卡片
function removeCard(card: { id: number }) {
  ElMessageBox.confirm("确定要删除此卡片吗?", "删除确认", {
    confirmButtonText: "确定",
    cancelButtonText: "取消",
    type: "warning"
  }).then(() => {
    containerLibrary.value.splice(
      containerLibrary.value.findIndex(c => c.id === card.id),
      1
    );
  });
}

function randomSelectionCard() {
  // 过滤出状态为 'read' 的项
  const filterContainerLibrary = containerLibrary.value.filter(item => item.status === "read");

  if (filterContainerLibrary.length === 0) {
    return undefined; // 如果数组为空，返回 undefined
  }

  // 提取独立的 content、title 和 tags 数组
  const contents = filterContainerLibrary.map(item => item.content);
  const titles = filterContainerLibrary.map(item => item.title);
  const tagsList = _.flattenDeep(filterContainerLibrary.map(item => item.tags));

  // 辅助函数：从数组中随机选择指定数量的元素
  const getRandomElements = (arr: string | _.NumericDictionary<any> | null | undefined, count = 1) => {
    if (!Array.isArray(arr) || arr.length === 0) return [];
    if (count <= 0) return [];

    // 如果需要的数量大于数组长度，则返回整个数组的随机排列
    if (count >= arr.length) {
      return _.shuffle(arr);
    }

    // 使用 lodash 的 sampleSize 方法随机选择指定数量的元素
    return _.sampleSize(arr, count);
  };

  // 随机选择内容、标题和标签
  const randomContent = getRandomElements(contents)[0];
  const randomTitle = getRandomElements(titles)[0];
  const randomTags: any = getRandomElements(tagsList, 5);

  // 检查是否成功获取所有需要的字段
  if (!randomContent || !randomTitle || randomTags.length !== 5) {
    return undefined; // 如果任一字段获取失败，返回 undefined
  }

  // 组合并赋值给 form.value
  form.value = {
    content: randomContent,
    title: randomTitle,
    tags: randomTags
  };
}
</script>

<template>
  <div>containerLibrary: {{ containerLibrary }}</div>
  <div>form: {{ form }}</div>
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
              <UniqueFileUploader @files-uploaded="handleFilesUploaded" />
            </div>

            <el-form-item label="标题">
              <el-input maxlength="30" v-model="form.title" placeholder="请输入标题" show-word-limit />
            </el-form-item>
            <el-form-item label="文案内容">
              <el-input
                type="textarea"
                maxlength="1000"
                :autosize="{ minRows: 4, maxRows: 4 }"
                v-model="form.content"
                show-word-limit
                placeholder="请输入文案内容"
              />
            </el-form-item>
            <el-form-item label="标签">
              <el-input-tag :max="9" v-model="form.tags" placeholder="请输入标签" />
              <el-alert
                type="info"
                style="margin-top: 12px;white-space: pre-line;"
                :closable="false"
                title="1、您可添加2~9个标签，按回车确认。部分平台最多显示5个标签，超出默认显示前5个标签。
                2、企鹅，b站，网易，搜狗，大风平台视频标签不能为空，企鹅至少2个标签，网易至少2个标签，网易至少3个标签"
              />
            </el-form-item>
          </el-form>
          <el-button
            type="primary"
            @click="randomSelectionCard()"
          >
            随机抽取卡片文案
          </el-button>
        </el-card>
      </div>
      <div class="right-platform">
        <!-- 平台组件 -->
        <div class="pingtai">
          <span class="pingtai-item" style="background-color: #fff;">平台</span>
          <span class="pingtai-item">抖音1</span>
          <span class="pingtai-item">抖音2</span>
          <span class="pingtai-item">抖音3</span>
          <span class="pingtai-item">抖音4</span>
        </div>
        <el-card class="pingtai-conter">
          <div style="margin-bottom: 20px;">
            数据存储库
          </div>
          <!-- 循环渲染多个卡片 -->
          <el-card v-for="card in containerLibrary" :key="card.id" class="card-item">
            <div>
              <el-form v-if="card.status === 'edit'" label-position="top" :model="card" label-width="80px">
                <el-form-item label="标题">
                  <el-input v-model="card.title" placeholder="请输入标题" />
                </el-form-item>
                <el-form-item label="文案内容">
                  <el-input
                    type="textarea"
                    :autosize="{ minRows: 4, maxRows: 4 }"
                    v-model="card.content"
                    placeholder="请输入文案内容"
                  />
                </el-form-item>
                <el-form-item label="标签">
                  <el-input-tag v-model="card.tags" placeholder="请输入标签" />
                </el-form-item>
              </el-form>
              <div v-if="card.status === 'read'" style="display: flex;flex-direction: column;">
                <div style="margin-bottom: 6px;">
                  <span>标题：</span> <span>{{ card.title }}</span>
                </div>
                <div style="margin-bottom: 6px;">
                  <span>文案内容：</span> <span>{{ card.content }}</span>
                </div>
                <div>
                  <span>标签：</span>
                  <el-tag style="margin-right: 6px;" v-for="(item, index) in card.tags" :key="index" type="info">
                    {{ item }}
                  </el-tag>
                </div>
              </div>
            </div>

            <!-- 卡片操作按钮组 -->
            <div style="display: flex; justify-content: end">
              <el-button
                type="primary"
                v-if="card.status === 'edit'"
                @click="saveCard(card)"
              >
                保存
              </el-button>
              <el-button
                type="warning"
                v-if="card.status === 'read'"
                @click="editCard(card)"
              >
                编辑
              </el-button>
              <el-button
                type="danger"

                @click="removeCard(card)"
              >
                删除
              </el-button>
            </div>
          </el-card>

          <!-- 添加卡片的按钮 -->
          <div style="display: flex; justify-content: center; align-items: center; margin-top: 20px; ">
            <div @click="addCard" style="cursor: pointer;display: flex;">
              <el-icon :size="20" color="#409EFF">
                <CirclePlus />
              </el-icon>
              <span style="margin-left: 8px;">添加卡片</span>
            </div>
          </div>
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
    overflow-y: auto;
    max-height: 80vh;
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
    height: 80vh;

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
      overflow-y: auto;
      padding-bottom: 72px;
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
