<script lang="ts" setup>
import { NIcon, useThemeVars } from 'naive-ui';

import { RouterLink, useRoute } from 'vue-router';
import { Home2, Menu2 } from '@vicons/tabler';

import { storeToRefs } from 'pinia';
import MenuLayout from '../components/MenuLayout.vue';
import NavbarButtons from '../components/NavbarButtons.vue';
import { useStyleStore } from '@/stores/style.store';
import { config } from '@/config';
import type { ToolCategory } from '@/tools/tools.types';
import { useToolStore } from '@/tools/tools.store';
import { useTracker } from '@/modules/tracker/tracker.services';
import CollapsibleToolMenu from '@/components/CollapsibleToolMenu.vue';

const themeVars = useThemeVars();
const styleStore = useStyleStore();
const route = useRoute();
const isHome = computed(() => route.path === '/');
const version = config.app.version;
const commitSha = config.app.lastCommitSha.slice(0, 7);

const { tracker } = useTracker();
const { t } = useI18n();

const toolStore = useToolStore();
const { favoriteTools, toolsByCategory } = storeToRefs(toolStore);

watch(isHome, (home) => {
  if (home) {
    styleStore.isMenuCollapsed = true;
  }
}, { immediate: true });

const tools = computed<ToolCategory[]>(() => [
  ...(favoriteTools.value.length > 0 ? [{ name: t('tools.categories.favorite-tools'), components: favoriteTools.value }] : []),
  ...toolsByCategory.value,
]);
</script>

<template>
  <MenuLayout class="menu-layout" :class="{ isSmallScreen: styleStore.isSmallScreen }">
    <template #sider>
      <RouterLink v-if="!isHome" to="/" class="hero-wrapper">
        <img src="/hero-bg.jpg" alt="hero background" class="hero-bg" />
        <div class="hero-overlay" />
        <div class="text-wrapper">
          <div class="title">
            一六工具箱
          </div>
          <div class="divider" />
          <div class="subtitle">
            {{ $t('home.subtitle') }}
          </div>
        </div>
      </RouterLink>

      <div v-if="!isHome" class="sider-content">
        <div v-if="styleStore.isSmallScreen" flex flex-col items-center>
          <locale-selector w="90%" />

          <div flex justify-center>
            <NavbarButtons />
          </div>
        </div>

        <CollapsibleToolMenu :tools-by-category="tools" />

        <div class="footer">
          <div>
            一六工具箱

            <c-link target="_blank" rel="noopener" :href="`https://github.com/yilaijiuzou-cmyk/it-tools/tree/v${version}`">
              v{{ version }}
            </c-link>

            <template v-if="commitSha && commitSha.length > 0">
              -
              <c-link
                target="_blank"
                rel="noopener"
                type="primary"
                :href="`https://github.com/yilaijiuzou-cmyk/it-tools/tree/${commitSha}`"
              >
                {{ commitSha }}
              </c-link>
            </template>
          </div>
          <div>
            基于
            <c-link target="_blank" rel="noopener" href="https://github.com/CorentinTh/it-tools">
              IT-Tools
            </c-link>
            开源项目构建
          </div>
        </div>
      </div>
    </template>

    <template #content>
      <div v-if="!isHome" flex items-center justify-center gap-2>
        <c-button
          circle
          variant="text"
          :aria-label="$t('home.toggleMenu')"
          @click="styleStore.isMenuCollapsed = !styleStore.isMenuCollapsed"
        >
          <NIcon size="25" :component="Menu2" />
        </c-button>

        <c-tooltip :tooltip="$t('home.home')" position="bottom">
          <c-button to="/" circle variant="text" :aria-label="$t('home.home')">
            <NIcon size="25" :component="Home2" />
          </c-button>
        </c-tooltip>

        <c-tooltip :tooltip="$t('home.uiLib')" position="bottom">
          <c-button v-if="config.app.env === 'development'" to="/c-lib" circle variant="text" :aria-label="$t('home.uiLib')">
            <icon-mdi:brush-variant text-20px />
          </c-button>
        </c-tooltip>

        <command-palette />

        <locale-selector v-if="!styleStore.isSmallScreen" />

        <div v-if="!styleStore.isSmallScreen">
          <NavbarButtons />
        </div>
      </div>
      <div :class="{ 'home-content': isHome }">
        <slot />
      </div>
    </template>
  </MenuLayout>
</template>

<style lang="less" scoped>
.footer {
  text-align: center;
  color: #838587;
  margin-top: 20px;
  padding: 20px 0;
}

.sider-content {
  padding-top: 160px;
  padding-bottom: 200px;
}

.home-content {
  padding: 0 !important;
  height: 100%;

  :deep(.n-layout-scroll-container) {
    padding: 0 !important;
  }
}

.hero-wrapper {
  position: absolute;
  display: block;
  left: 0;
  width: 100%;
  z-index: 10;
  overflow: hidden;

  .hero-bg {
    width: 100%;
    height: 160px;
    object-fit: cover;
    display: block;
  }

  .hero-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 160px;
    background: linear-gradient(
      180deg,
      rgba(0, 0, 0, 0.1) 0%,
      rgba(0, 0, 0, 0.5) 100%
    );
  }

  .text-wrapper {
    position: absolute;
    left: 0;
    width: 100%;
    text-align: center;
    top: 16px;
    color: #fff;

    .title {
      font-size: 25px;
      font-weight: 600;
      text-shadow: 0 2px 8px rgba(0, 0, 0, 0.4);
    }

    .divider {
      width: 50px;
      height: 2px;
      border-radius: 4px;
      background-color: #fff;
      margin: 0 auto 5px;
    }

    .subtitle {
      font-size: 16px;
      text-shadow: 0 1px 4px rgba(0, 0, 0, 0.4);
    }
  }
}
</style>
