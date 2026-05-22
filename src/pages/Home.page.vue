<script setup lang="ts">
import { IconDragDrop } from '@tabler/icons-vue';
import { useHead } from '@vueuse/head';
import { computed } from 'vue';
import Draggable from 'vuedraggable';
import ToolCard from '../components/ToolCard.vue';
import { useToolStore } from '@/tools/tools.store';
import { config } from '@/config';

const toolStore = useToolStore();
const isLandingSite = computed(() => config.app.siteMode === 'home');

useHead({ title: isLandingSite.value ? '一六工具箱 - 高效便捷，触手可及' : '一六工具箱 - 开发者的效率利器' });
const { t } = useI18n();

const favoriteTools = computed(() => toolStore.favoriteTools);

function onUpdateFavoriteTools() {
  toolStore.updateFavoriteTools(favoriteTools.value);
}
</script>

<template>
  <div v-if="isLandingSite" class="landing">
    <section class="hero">
      <div class="hero-bg" />
      <div class="hero-overlay" />
      <div class="hero-content">
        <h1 class="hero-title">一六工具箱</h1>
        <div class="hero-line" />
        <p class="hero-slogan">{{ $t('home.subtitle') }}</p>
      </div>
    </section>

    <section class="tools-section">
      <div class="tools-grid">
        <a href="https://ihgpoints.yilaijiuzou.com" target="_blank" class="tool-banner banner-link">
          <svg class="banner-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
            <circle cx="12" cy="12" r="10" />
            <path d="M8 14c1.5-2 2.5-3 4-3s2.5 1 4 3" />
            <line x1="12" y1="7" x2="12" y2="10" />
            <circle cx="12" cy="17" r="0.5" fill="currentColor" stroke="none" />
          </svg>
          <h3 class="banner-title">IHG 积分评估</h3>
          <p class="banner-desc">积分购买价值分析计算</p>
        </a>
        <div class="tool-banner">
          <svg class="banner-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
            <path d="M14.7 6.3a1 1 0 0 0 0 1.4l1.6 1.6a1 1 0 0 0 1.4 0l3.77-3.77a6 6 0 0 1-7.94 7.94l-6.91 6.91a2.12 2.12 0 0 1-3-3l6.91-6.91a6 6 0 0 1 7.94-7.94l-3.76 3.76z" />
          </svg>
          <h3 class="banner-title">常用工具</h3>
          <p class="banner-desc">日常开发工具集合</p>
        </div>
        <div class="tool-banner">
          <svg class="banner-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
            <circle cx="12" cy="12" r="10" />
            <line x1="12" y1="8" x2="12" y2="16" />
            <line x1="8" y1="12" x2="16" y2="12" />
          </svg>
          <h3 class="banner-title">更多工具</h3>
          <p class="banner-desc">更多工具陆续上线</p>
        </div>
      </div>
    </section>
  </div>

  <div v-else class="pt-50px">
    <div class="grid-wrapper">
      <transition name="height">
        <div v-if="toolStore.favoriteTools.length > 0">
          <h3 class="mb-5px mt-25px text-neutral-400 font-500">
            {{ $t('home.categories.favoriteTools') }}
            <c-tooltip :tooltip="$t('home.categories.favoritesDndToolTip')">
              <n-icon :component="IconDragDrop" size="18" />
            </c-tooltip>
          </h3>
          <Draggable
            :list="favoriteTools"
            class="grid grid-cols-1 gap-12px lg:grid-cols-3 md:grid-cols-3 sm:grid-cols-2 xl:grid-cols-4"
            ghost-class="ghost-favorites-draggable"
            item-key="name"
            @end="onUpdateFavoriteTools"
          >
            <template #item="{ element: tool }">
              <ToolCard :tool="tool" />
            </template>
          </Draggable>
        </div>
      </transition>

      <div v-if="toolStore.newTools.length > 0">
        <h3 class="mb-5px mt-25px text-neutral-400 font-500">
          {{ t('home.categories.newestTools') }}
        </h3>
        <div class="grid grid-cols-1 gap-12px lg:grid-cols-3 md:grid-cols-3 sm:grid-cols-2 xl:grid-cols-4">
          <ToolCard v-for="tool in toolStore.newTools" :key="tool.name" :tool="tool" />
        </div>
      </div>

      <h3 class="mb-5px mt-25px text-neutral-400 font-500">
        {{ $t('home.categories.allTools') }}
      </h3>
      <div class="grid grid-cols-1 gap-12px lg:grid-cols-3 md:grid-cols-3 sm:grid-cols-2 xl:grid-cols-4">
        <ToolCard v-for="tool in toolStore.tools" :key="tool.name" :tool="tool" />
      </div>
    </div>
  </div>
</template>

<style scoped lang="less">
.landing {
  width: 100%;
  min-height: 100vh;
}

.hero {
  position: relative;
  width: 100%;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.hero-bg {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url('/hero-bg.jpg');
  background-size: cover;
  background-position: center;
  filter: brightness(0.75);
}

.hero-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    180deg,
    rgba(0, 0, 0, 0.3) 0%,
    rgba(0, 0, 0, 0.5) 60%,
    rgba(0, 0, 0, 0.7) 100%
  );
}

.hero-content {
  position: relative;
  z-index: 2;
  text-align: center;
  color: #fff;
}

.hero-title {
  font-size: 52px;
  font-weight: 300;
  letter-spacing: 12px;
  margin: 0;
  text-shadow: 0 4px 20px rgba(0, 0, 0, 0.4);
}

.hero-line {
  width: 50px;
  height: 1px;
  background: rgba(255, 255, 255, 0.5);
  margin: 24px auto;
}

.hero-slogan {
  font-size: 18px;
  font-weight: 300;
  letter-spacing: 4px;
  color: rgba(255, 255, 255, 0.85);
  text-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
}

.tools-section {
  padding: 80px 26px 100px;
  background: #0f1419;
}

.tools-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
  max-width: 900px;
  margin: 0 auto;
}

.tool-banner {
  background: rgba(255, 255, 255, 0.04);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 12px;
  padding: 40px 20px;
  text-align: center;
  transition: border-color 0.3s ease, background 0.3s ease;
  cursor: default;
  text-decoration: none;
  display: block;
  color: inherit;

  &:hover {
    border-color: rgba(255, 255, 255, 0.2);
  }
}

.banner-link {
  cursor: pointer;

  &:hover {
    border-color: rgba(255, 255, 255, 0.3);
    background: rgba(255, 255, 255, 0.07);
  }
}

.banner-icon {
  width: 36px;
  height: 36px;
  margin: 0 auto 16px;
  display: block;
  color: rgba(255, 255, 255, 0.25);
  transition: color 0.3s ease;
}

.banner-title {
  font-size: 18px;
  font-weight: 500;
  color: rgba(255, 255, 255, 0.7);
  margin: 0 0 8px;
}

.banner-desc {
  font-size: 14px;
  color: rgba(255, 255, 255, 0.35);
  margin: 0;
}

.height-enter-active,
.height-leave-active {
  transition: all 0.5s ease-in-out;
  overflow: hidden;
  max-height: 500px;
}

.height-enter-from,
.height-leave-to {
  max-height: 42px;
  overflow: hidden;
  opacity: 0;
  margin-bottom: 0;
}

.ghost-favorites-draggable {
  opacity: 0.4;
  background-color: #ccc;
  border: 2px dashed #666;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  transform: scale(1.1);
  animation: ghost-favorites-draggable-animation 0.2s ease-out;
}

@keyframes ghost-favorites-draggable-animation {
  0% {
    opacity: 0;
    transform: scale(0.9);
  }
  100% {
    opacity: 0.4;
    transform: scale(1);
  }
}
</style>
