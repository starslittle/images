<template>
  <div class="card-list-container">
    <a-button @click="toggleSelectAll" class="select-all-button">
      {{ isAllSelected ? '取消全选' : '全选' }}
    </a-button>
    <a-list
      :grid="{ gutter: 16, xs: 1, sm: 2, md: 4, lg: 5, xl: 5, xxl: 5 }"
      :data-source="cardData"
    >
      <template #renderItem="{ item }">
        <a-list-item>
          <a-card hoverable class="custom-card" @click="toggleSelection(item)">
            <div class="card-wrapper">
              <div class="image-container">
                <a-checkbox
                  class="select-checkbox"
                  :checked="item.selected"
                  @click.stop
                ></a-checkbox>
                <a-image :src="item.imageUrl" :preview="false" class="card-image" />
                <div class="eye-icon" @click.stop="openPreview(item.imageUrl)">
                  <EyeOutlined />
                </div>
              </div>
              <div class="text-content">
                <p>{{ item.text }}</p>
              </div>
            </div>
          </a-card>
        </a-list-item>
      </template>
    </a-list>
    <a-image
      :width="200"
      :style="{ display: 'none' }"
      :preview="{
        visible: previewState.visible,
        onVisibleChange: setPreviewVisible,
        src: previewState.src,
      }"
      :src="previewState.src"
    />
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import {
  List as AList,
  Card as ACard,
  Image as AImage,
  Button as AButton,
  Checkbox as ACheckbox,
} from 'ant-design-vue';
import { EyeOutlined } from '@ant-design/icons-vue';

const AListItem = AList.Item;

interface CardDataItem {
  id: number;
  imageUrl: string;
  text: string;
  selected: boolean;
}

const cardData = ref<CardDataItem[]>([]);

for (let i = 1; i <= 20; i++) {
  cardData.value.push({
    id: i,
    imageUrl: `https://picsum.photos/seed/${i}/400/300`,
    text:
      i % 3 === 0
        ? `这是第 ${i} 张卡片的描述文字，这段文字比较长，用来测试多行文本的情况。`
        : `卡片 ${i} 的描述`,
    selected: false,
  });
}

const isAllSelected = computed(() => cardData.value.every((item) => item.selected));

const toggleSelectAll = () => {
  const targetState = !isAllSelected.value;
  cardData.value.forEach((item) => (item.selected = targetState));
};

const toggleSelection = (item: CardDataItem) => {
  item.selected = !item.selected;
};

const previewState = ref({
  visible: false,
  src: '',
});

const openPreview = (src: string) => {
  previewState.value.src = src;
  previewState.value.visible = true;
};

const setPreviewVisible = (visible: boolean) => {
  previewState.value.visible = visible;
};
</script>

<style scoped>
.card-list-container {
  padding: 24px;
  position: relative;
}

.select-all-button {
  margin-bottom: 16px;
}

.custom-card {
  height: 180px;
  overflow: hidden;
}

.custom-card :deep(.ant-card-body) {
  padding: 0;
  height: 100%;
}

.card-wrapper {
  display: flex;
  flex-direction: column;
  height: 100%;
}

.select-checkbox {
  position: absolute;
  top: 8px;
  left: 8px;
  z-index: 2;
  background-color: rgba(255, 255, 255, 0.7);
  border-radius: 4px;
}

.image-container {
  position: relative;
  flex-grow: 1; /* 让图片容器占据剩余空间 */
  min-height: 0; /* 允许 flex item 收缩 */
  overflow: hidden;
}

.card-image {
  width: 100%;
  height: 100%;
  object-fit: contain; /* 保证图片完整显示 */
}

.eye-icon {
  position: absolute;
  top: 0px;
  right: 3px;
  font-size: 24px;
  color: rgb(255, 254, 254);
  /* background-color: rgba(0, 0, 0, 0.5); */
  /* border-radius: 50%; */
  padding: 5px;
  cursor: pointer;
  z-index: 1;
}

.eye-icon:hover {
  color: #b5b8bc; /* 悬停时变为蓝色 */
}

.text-content {
  padding: 4px 12px; 
  flex-shrink: 0; 
  background-color: #fff;
  font-size: 12px;
  display: flex;
  flex-direction: column; 
  align-items: center;
  justify-content: center;
  min-height: 24px; 
}

.text-content p {
  margin: 0;
}
</style> 