<template>
  <div id="app-container">
    <ImageCardList />
  </div>
</template>

<script setup lang="ts">
import ImageCardList from './components/ImageCardList.vue';
</script>

<style>
html,
body,
#app {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
}

#app-container {
  width: 1280px; /* 您可以根据需要调整此宽度 */
  height: 960px; /* 您可以根据需要调整此高度 */
  margin: 0 auto; /* 水平居中 */
  position: relative;
  border: 1px solid #ccc; /* 添加边框以方便查看范围 */
  overflow: auto; /* 如果内容超出则显示滚动条 */
}
</style>

<style scoped>
</style>
