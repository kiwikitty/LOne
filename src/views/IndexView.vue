<template>
  <div class="common-layout">
    <el-container>
      <el-aside width="64px" :style="{ height: windowHeight + 'px' }">
        <div class="block" style="text-align: center; margin: 20px 0">
          <el-avatar :size="40" src="https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png" />
        </div>
        <el-menu default-active="2" class="el-menu-vertical" :collapse="true" ref="menuRef">
          <el-menu-item v-for="item in menuItems" :key="item.index" :index="item.index">
            <el-icon><setting /></el-icon>
            <div class="desc">{{ item.title }}</div>
          </el-menu-item>
        </el-menu>
      </el-aside>
      <el-container>
        <el-header class="contain-header">
          <el-icon><Star /></el-icon>
        </el-header>
        <el-main class="contain-main">
          <MainPage />
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script lang="ts" setup>
import { Star, Setting } from '@element-plus/icons-vue'
import { ref, onMounted, onBeforeUnmount, onUpdated, nextTick } from 'vue'
import Sortable from 'sortablejs'
import MainPage from './pages/MainPage.vue'
const windowHeight = ref(window.innerHeight)
const menuItems = ref([
  { index: '1', title: '菜单1' },
  { index: '2', title: '菜单2' },
  { index: '3', title: '菜单3' }
])
const menuRef = ref<any>(null)

onMounted(async () => {
  window.addEventListener('resize', handleResize)
  await nextTick()
  if (menuRef.value) {
    const menuDOM = menuRef.value.$el
    Sortable.create(menuDOM, {
      animation: 150,
      onEnd: ({ newIndex, oldIndex }) => {
        if (newIndex && oldIndex) {
          const movedItem = menuItems.value.splice(oldIndex, 1)[0]
          menuItems.value.splice(newIndex!, 0, movedItem)
        }
        // 这里可以添加其他逻辑，比如更新UI状态或者发送请求到服务器
      }
    })
  }
})

// 在组件卸载前移除事件监听器
onBeforeUnmount(() => {
  window.removeEventListener('resize', handleResize)
})

// 当窗口大小改变时更新高度
function handleResize() {
  windowHeight.value = window.innerHeight
}

// 更新时也重新计算高度
onUpdated(() => {
  handleResize()
})
</script>

<style lang="less" scoped>
.el-menu {
  border-right: none;
}
.el-menu-vertical {
  background-color: transparent;
}
.el-menu-item {
  color: #e2e0e0;
  transition: transform 0.3s ease;
  flex-direction: column;
  justify-content: space-evenly;
  .desc {
    line-height: 12px;
    height: 12px;
    font-size: 12px;
  }
}
.el-menu-item.is-active {
  color: #f5f5f5;
  background-color: rgba(255, 255, 255, 0.1);
}
.el-menu-item:hover {
  background-color: transparent;
  transform: scale(1.2);
}
.contain-header {
  text-align: right;
  line-height: 60px;
}
</style>
