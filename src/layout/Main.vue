<template>
  <div class="main">
    <div class="header">
      <img class="logo" src="@/assets/vue.svg" />
      <div class="home item">{{ APP_INFO.title }}</div>
      <div v-if="route.meta.title" class="title">
        {{ route.meta.title || '' }}
      </div>
      <div class="flex-1"></div>
      <!-- <div class="item">个人中心</div> -->
      <el-dropdown class="item" @command="handleCommand">
        操作
        <template #dropdown>
          <el-dropdown-menu>
            <!-- <el-dropdown-item command="ModifyPassword">
              修改密码
            </el-dropdown-item> -->
            <el-dropdown-item command="Logout">
              <span class="color-red"> 退出登录 </span>
            </el-dropdown-item>
          </el-dropdown-menu>
        </template>
      </el-dropdown>
    </div>
    <div class="content flex flex-1">
      <Menu />
      <div class="content-page flex-1">
        <router-view></router-view>
      </div>
    </div>
  </div>
</template>

<script setup>
import { useRoute, useRouter } from 'vue-router';
import { APP_INFO, CACHE_PREFIX, CACHE_AUTH_PREFIX } from '@/config/base';
import Menu from '@/components/menu/index.vue';
import { removeCache } from '@/utils/common.js';
import { removeAuthToken, clearAuth } from '@/utils/auth.js';
import { ElMessage, ElMessageBox } from 'element-plus';

const route = useRoute();
const router = useRouter();

// 处理操作
const handleCommand = async (command) => {
  switch (command) {
    case 'ModifyPassword':
      break;

    case 'Logout':
      {
        try {
          await ElMessageBox.confirm('确定退出登录？', {
            title: '操作提示',
            type: 'warning',
            closeOnClickModal: false,
          });
          // removeAuthToken();
          clearAuth();
          ElMessage.success('退出登录成功');
          router.replace({ name: 'Login' });
        } catch (error) {
          if (error === 'cancel') return;
          ElMessage.error(`${error}`);
        }
      }
      break;

    default:
      break;
  }
};
</script>

<style lang="scss" scoped>
.main {
  overflow: hidden;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
}
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 56px;
  padding: 0 16px;
  color: #fff;
  font-size: 16px;
  line-height: 56px;
  background-color: $colorPrimary;
  background-image: linear-gradient(
    to left,
    rgba(0, 0, 0, 0.4),
    rgba(0, 0, 0, 0) 50%
  );
  .logo {
    max-height: 100%;
    max-width: 100%;
  }

  .item {
    padding: 0 16px;
    cursor: pointer;
    font-size: 14px;
    &.el-dropdown {
      color: #fff;
    }
  }

  .home {
    font-weight: bold;
    font-size: 18px;
  }
  .title {
    width: 140px;
    padding: 0 16px;
    font-weight: bold;
  }
}

.content {
  display: flex;
  overflow: hidden;
  height: calc(100% - 56px);
  .content-page {
    width: calc(100% - 200px);
    margin: 8px;
    overflow: hidden;
  }
}
</style>
