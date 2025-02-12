<script setup lang="ts">
import { ref } from "vue";
import { useRouter } from "vue-router";
import { MenuOutlined, MenuUnfoldOutlined } from "@ant-design/icons-vue";

import { i18nRender } from "@/locales";
import { getMenus } from "@/plugin/asyncRoute";

const selectedKeys = ref<Array<string>>([]);
const collapsed = ref<boolean>(false);

const router = useRouter();
const menus = getMenus();

// eslint-disable-next-line @typescript-eslint/no-explicit-any
const toRoute = (menu: any) => {
  router.push(menu.path);
};
</script>

<template>
  <a-layout class="layout">
    <!-- 侧边栏 -->
    <a-layout-sider
      v-model:collapsed="collapsed"
      :trigger="null"
      class="sider"
      collapsible
    >
      <div class="logo">E-Show</div>

      <!-- 菜单导航栏 -->
      <a-menu v-model:selectedKeys="selectedKeys" theme="dark" mode="inline">
        <a-sub-menu v-for="menu in menus" :key="menu.id">
          <template #title>
            <component :is="menu.icon"></component>
            <span>{{ i18nRender(`stage.${menu.id}.title`) }}</span>
          </template>
          <template v-if="menu.children">
            <a-menu-item
              v-for="subMenu in menu.children"
              :key="subMenu.id"
              @click="toRoute(subMenu)"
            >
              {{ i18nRender(`stage.${subMenu.id}.title`) }}
            </a-menu-item>
          </template>
        </a-sub-menu>
      </a-menu>
    </a-layout-sider>

    <a-layout>
      <!-- 头部信息显示 -->
      <a-layout-header class="header" style="">
        <MenuUnfoldOutlined
          v-if="collapsed"
          class="trigger"
          @click="() => (collapsed = !collapsed)"
        />
        <MenuOutlined
          v-else
          class="trigger"
          @click="() => (collapsed = !collapsed)"
        />
      </a-layout-header>

      <!-- 主画面显示区 -->
      <a-layout-content
        :style="{
          margin: '24px 16px',
          padding: '24px',
          background: '#fff',
          minHeight: '280px',
        }"
      >
        <router-view />
      </a-layout-content>
    </a-layout>
  </a-layout>
</template>

<style lang="less">
.layout {
  height: 100%;
  min-height: 680px;
  background: #fff;
  .sider {
    .logo {
      height: 32px;
      background: rgba(255, 255, 255, 0.3);
      margin: 16px;
      color: #fff;
      text-align: center;
      line-height: 32px;
    }
    .logo:hover {
      color: #1890ff;
    }
  }
  .header {
    background: #fff;
    padding: 0;
    .trigger {
      font-size: 18px;
      line-height: 64px;
      padding: 0 24px;
      cursor: pointer;
      transition: color 0.3s;
    }
    .trigger:hover {
      color: #1890ff;
    }
    .language {
      border: 1px solid #000;
      color: #000;
      display: inline-block;
      // float: right;
      box-sizing: border-box;
      width: 72px;
      height: 28px;
      line-height: 28px;
      margin: auto auto;
      margin-right: 40px;
      text-align: center;
      border-radius: 2px;
      cursor: pointer;
      font-family: PingFangSC-Medium;
      font-size: 16px;
      opacity: 0.6;
    }
  }
}
</style>
